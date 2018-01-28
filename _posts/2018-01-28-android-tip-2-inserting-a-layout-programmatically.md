---
title: 'Android Tips 2: Inserting and removing a layout programmatically'
layout: post
image: https://postimg.org/image/vefxherup/
feature-img: assets/img/unsplash/coding.jpg
tags:
- Android
- Tips
keywords: Android, Custom Views, Android tips, Insert layout, Programmatically
lang: en
category: articles
publisher: CodeAndWave
---

There are times in our app where we show different forms on the user depeding on the selection. Usually what we do is that we just set the visibility to gone. While this works perfectly, the pain comes when designing it on XML. To design the desired view, you have to set visibilty the of first form for example to VISIBLE and the set it to GONE then design the next form.  What if we don't have to worry about setting the visibility of the object when designing? Fortunately there is another way. Inserting and removing layout programmatically.

### Inserting a layout

To insert a layout, all we need to do is to make sure that we have declared the ID of the container layout(*in our case our main activity layout root which is **layoutInserPoint***). Then design the layout that will be inserted to the container.

activitymain.xml- our Acitivty main layout

``` java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/layoutInserPoint"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.example.skadush.custom_layout.MainActivity">



    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|center_horizontal"
        android:onClick="addView"
        android:text="addView" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|center_horizontal"
        android:onClick="removeView"
        android:text="removeView" />

</LinearLayout>
```

insertionlayot.xml - Layout to be inserted
``` java
<TextView android:text="sample"
    android:id="@+id/txtsampleinsertion"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    xmlns:android="http://schemas.android.com/apk/res/android" />

</android.support.v7.widget.LinearLayoutCompat>
```

Then we need to inflate the layout that is to be inserted to the container using `LayoutInflater`
``` java
    LayoutInflater vi = (LayoutInflater) getApplicationContext().getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        View v = vi.inflate(R.layout.insertionlayout, null);
```



We then call the `addView()` of the container viewgroup

 ``` java
 container = (ViewGroup) findViewById(R.id.layoutInserPoint);
   container.addView(v, 0, new ViewGroup.LayoutParams(ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT));
```

BAAAM! We are done.  Layout inserted. How about removing? How do we remove a layout?

To remove a view from the container we have 3 options:
* `removeView`
* `removeAllViews`
* `removeViewAt`

Depending on what you do, you can remove all the views, or remove view by index, or remove view by specific view. Since I like to be more specific, I use the remove view by view. This way I could avoid accidentally removing some UI elements like for example a button.
 ``` java
 container.removeView(textViewList.get(textViewList.size() - 1));
```

Here is the full java code:
``` java
public class MainActivity extends AppCompatActivity {

  ViewGroup container;
    TextView textView;
    List<TextView> textViewList = new ArrayList<>();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

    }

    public void addView(View view) {

        LayoutInflater vi = (LayoutInflater) getApplicationContext().getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        View v = vi.inflate(R.layout.insertionlayout, null);


         textView = (TextView) v.findViewById(R.id.txtsampleinsertion);
        textView.setText("Sample Text Added");


        container = (ViewGroup) findViewById(R.id.layoutInserPoint);
        container.addView(v, 0, new ViewGroup.LayoutParams(ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT));
        textViewList.add(textView);

    }

    public void removeView(View view) {

        if(textViewList.size() > 0){
            container.removeView(textViewList.get(textViewList.size() - 1));

            textViewList.remove(textViewList.size()- 1);
        }

    }
}
```

I believe that this approach is much more cleaner compared to Hiding and Showing a view in a layout. This way you can track bugs easily.