---
title: 'Android Tips 1: Custom Views'
layout: post
image: https://postimg.org/image/vefxherup/
feature-img: assets/img/unsplash/coding.jpg
tags:
- Android
- Tips
keywords: Android, Custom Views, Android tips
lang: en
category: articles
publisher: CodeAndWave
---

While android default views are helpful, sometimes we want to have our own custom views.In this example we'll make a custom textview. Let's get started

To make a custom view, we must first declare a styleable attribute on our **attrs.xml**

``` xml
 <declare-styleable name="UnderlinedTextView" >
        <attr name="underlineWidth" format="dimension" />
        <attr name="underlineColor" format="color" />
 </declare-styleable>
```

Then we create a class that extends the TextView Object


``` java
public class CustomTextView extends AppCompatTextView {

    private float mStrokeWidth;


    public CustomTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs, defStyleAttr);
    }

    void init(Context context, AttributeSet attributeSet, int defStyle){
        TypedArray typedArray = context.obtainStyledAttributes(attributeSet, R.styleable.UnderlinedTextView, defStyle, 0);
        int underlineColor = typedArray.getColor(R.styleable.UnderlinedTextView_underlineColor, 0xFFFF0000);
        mStrokeWidth = typedArray.getDimension(R.styleable.UnderlinedTextView_underlineWidth, getWidth());
        typedArray.recycle();
    }

    @Override
    protected void onDraw(Canvas canvas) {

        super.onDraw(canvas);
    }
}
```

Now if you check our styleable, we don't have a styleable named like this `R.styleable.UnderlinedTextView_underlineColor`. So where did we get that? Well that is because after the *underline* is the item we declared.

[![tip1]({{site.baseurl}}/assets/img/blog/tip1.png)]({{site.baseurl}}/assets/img/blog/tip1.png)

Then we draw a line on the `onDraw() `using **Paint**

``` java
canvas.drawLine(0,getHeight(),getWidth(),getHeight(),mPaint);
```

Then baam! we now have a custom textview that will draw a line at the bottom of the text

[![tip1]({{site.baseurl}}/assets/img/blog/sstips1.png)]({{site.baseurl}}/assets/img/blog/sstips1.png)

Here is the full source


``` java
public class CustomTextView extends AppCompatTextView {

    private float mStrokeWidth;
    private Paint mPaint;

    public CustomTextView(Context context) {
        this(context, null, 0);
    }

    public CustomTextView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    public CustomTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs, defStyleAttr);
    }

    void init(Context context, AttributeSet attributeSet, int defStyle){
        TypedArray typedArray = context.obtainStyledAttributes(attributeSet, R.styleable.UnderlinedTextView, defStyle, 0);
        int underlineColor = typedArray.getColor(R.styleable.UnderlinedTextView_underlineColor, 0xFFFF0000);
        mStrokeWidth = typedArray.getDimension(R.styleable.UnderlinedTextView_underlineWidth, getWidth());
        typedArray.recycle();



        mPaint = new Paint();
        mPaint.setStyle(Paint.Style.STROKE);
        mPaint.setColor(underlineColor);
        mPaint.setStrokeWidth(mStrokeWidth);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        canvas.drawLine(0,getHeight(),getWidth(),getHeight(),mPaint);
        super.onDraw(canvas);

    }
}
```
