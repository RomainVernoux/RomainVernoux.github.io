---
layout: post
title: "Parcels and inheritance in Android"
date: 2016-04-30
categories: java android
tags: english
---
To pass information between Activities and save states during the application lifecycle, Android uses `Parcel` objects.
Like `Serializable` in Java, the `Parcelable` interface offers a marshalling mechanism to represent and transfer object instances as bytes.
In this post I will present some of the differences between `Parcels` and `Serializable`, then explain how to implement the `Parcelable` interface correctly, in particular in a hierarchy of inherited classes.

<!--more-->

## Why Parcel?

Parcels are a lightweight alternative to `Serializable` for inter-process communication.
As stated in the [documentation](http://developer.android.com/reference/android/os/Parcel.html), it is not a general purpose serialization mechanism and should not be used to store object representations on the disk.
Indeed, unlike `Serializable` and its `serialVersionUID` field, `Parcelable` does not offer a versioning mechanism nor any guarantee that its underlying implementation will not change.

The main advantage of `Parcelable` over `Serializable` is performance: instead of using reflection to automatically write all object fields into a byte stream, classes implementing `Parcelable` need to explicitely specify how they are converted and restored.
As explained [here](http://www.developerphil.com/parcelable-vs-serializable/) and [there](http://www.3pillarglobal.com/insights/parcelable-vs-java-serialization-in-android-app-development), the performance gain can be significant.


## How to Parcel?

A `Parcelable` class `MyClass` must implement two methods, `int describeContents()` and more importantly `void writeToParcel(Parcel out, int flags)`, but also have a static final field called `CREATOR` and implementing `Parcelable.Creator<MyClass>`.
Since `Parcelable.Creator` is an interface, `CREATOR` will have to provide two other methods, `MyClass[] newArray(int size)` and more importantly `MyClass createFromParcel(Parcel in)`.
As explained above, these methods specify how a `MyClass` instance should be written to a `Parcel` and restored from one.
A basic implementation usually looks like the following.

{% highlight java %}
public class MyClass implements Parcelable {

    private int mInt;
    private long mLong;
    
    public int describeContents() {
        return 0;
    }
    
    public void writeToParcel(Parcel out, int flags) {
        out.writeInt(mInt);
        out.writeLong(mLong);
    }
    
    public static final Parcelable.Creator<MyClass> CREATOR = new Parcelable.Creator<MyClass>() {
        public MyClass createFromParcel(Parcel in) {
            return new MyClass(in);
        }
    
        public MyClass[] newArray(int size) {
            return new MyClass[size];
        }
    };
    
    private MyClass(Parcel in) {
        mInt = in.readInt();
        mLong = in.readLong();
    }
}
{% endhighlight %}

At first, it may look surprising that even though `Parcelable` is an interface, it does not only rely on the type system to ensure that its child classes implement the required methods, but also on the *convention* that they contain a field with a given name and a given type.
In my opinion, this is not very elegant of a design choice, but looking deeper into the requirements, there was no credible alternative to begin with.

Indeed, a `Parcelable` object needs to provide some constructor to create a new instance from a `Parcel`.
If this constructor were a static factory method, its presence could not be enforced by the interface (in Java, static methods are not inherited).
If this constructor were a public constructor, its presence could not be enforced by the interface either (in Java interfaces, all constructors are private).
In fact, the only way to be sure of the presence of a particular constructor would be for `Parcelable` to be an abstract class.
But Java does not support multiple inheritance, so that would be too much of a constraint.
Finally, keep in mind that `Parcelable` aims for performance, therefore any of the reflection-based tricks that are used in many of today's Java frameworks are probably off the table.


## How to Parcel in subclasses?

Now, given the particular `Parcelable` contract, making an entire class hierarchy `Parcelable` can get quite tricky.
Consider for instance an interface `Message` implemented by an `AbstractMessage` and some classes extending `AbstractMessage`, including `BasicMessage`.

Since we want any member of the class hierarchy to be convertible to a `Parcel`, `Message` itself should extend `Parcelable`, and any parent class should be able to write/read its own fields to/from the `Parcel` when a child class is converted or restored.
In practice, the code is as follows.


{% highlight java %}
public interface Message extends Parcelable {
    // Other interface methods here
}
{% endhighlight %}


{% highlight java %}
public abstract class AbstractMessage implements Message {

    private int mInt;
    
    // Rest of the class code here
    
    @Override
    public void writeToParcel(Parcel out, int flags) {
        out.writeInt(mInt);
    }
    
    protected AbstractMessage(Parcel in) {
        mInt = in.readInt();
    }
}
{% endhighlight %}


{% highlight java %}
public class BasicMessage extends AbstractMessage {

    private long mLong;
    
    // Rest of the class code here
    
    public int describeContents() {
        return 0;
    }
    
    @Override
    public void writeToParcel(Parcel out, int flags) {
        super.writeToParcel(out, flags);
        out.writeLong(mLong);
    }
    
    public static final Parcelable.Creator<BasicMessage> CREATOR = new Parcelable.Creator<BasicMessage >() {
    
        public BasicMessage createFromParcel(Parcel in) {
            return new BasicMessage(in);
        }
        
        public BasicMessage[] newArray(int size) {
            return new BasicMessage[size];
        }
    };
    
    protected BasicMessage(Parcel in) {
        super(in);
        mLong = in.readLong();
    }
}
{% endhighlight %}

Notice in particular how the child class calls its parent's methods when it is written to a `Parcel` and restored from one.

Another important point is that the `CREATOR` object must be present in all classes that will be converted or restored at runtime (in our case, all children of `AbstractMessage`).
In this case, the corresponding `Parcelable.Creator` implementation needs to be duplicated.
This is because the field is static and therefore cannot be moved to `AbstractMessage` and inherited from it.

I believe there is no elegant workaround to this issue, but share yours if you found one!
