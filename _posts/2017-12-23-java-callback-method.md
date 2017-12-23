---
title: Java - CallBack Method
layout: post
image: https://postimg.org/image/vefxherup/
feature-img: assets/img/unsplash/coding.jpg
tags:
- Java
- OOP
keywords: Java, OOP, Inheritance
lang: en
category: articles
publisher: CodeAndWave
---

I learned about callback when I started learning fragments in android which is use to communicate between fragments and activity.  Though Callback originally came back in the days of c/c++, we can do the same thing in Java using [interface]({% post_url 2017-11-25-java-interface %}). 

### What is a callback method?

A callback method is a way to **notify or inform** a class that a task  in another class is done. In plain english example, a conversation between two guy would be like this

<p class="callback">
 Dude1<br/>
&nbsp; &nbsp; Hey man, wake me up when september ends*.<br/>
Dude2<br/>
&nbsp; &nbsp; Sure thing bro! <br/>
&nbsp;  ................... Task being done here.................<br/>
 Dude2 to Dude1<br/>
&nbsp; &nbsp; Wake up, September has ended.<br/>
</p>

Well, that's how I imagine it.  Anyway lets see some code example first then see what is going on in the code.

### How to create a Callback Method?
To have a callback method, we need to use an [interface]({% post_url 2017-11-25-java-interface %})

``` java
public interface IOnClickEvent {
    void onClick();
}

class ClickListener {

    IOnClickEvent iOnClickEvent;

    ClickListener(IOnClickEvent iOnClickEvent) {
        this.iOnClickEvent = iOnClickEvent;
        CallClick();
    }

     private void CallClick(){
        System.out.println("Doing Some work here ...........");
        iOnClickEvent.onClick();
    }
}
public class Button implements IOnClickEvent {

    private ClickListener clickListener;

    void CallEvent(){
        System.out.println("Initial Call");
        clickListener = new ClickListener(this);   // polymorphism in action right here!
    }


    @Override
    public void onClick() {
        System.out.println("Called Back");
    }
}
public class Main {
    public static void main(String[] args) {

        Button b = new Button();
        b.CallEvent();
    }
}
```

Ouput:
```
Initial Call
Doing Some work here ...........
Called Back
```



### The fun stuff
When we call the CallEvent() what is happening is that we instantiate the **`clickListener`**  variable by passing the **[this]({% post_url 2017-11-12-Java-This-Keyword %})** keyword. This is **[polymorphism]({% post_url 2017-11-29-java-polymorphism %})** in action by the way. Since the Button class implements the IOnClickEvent **[interface]({% post_url 2017-11-25-java-interface %})**, we can pass "*`this`*". When the constructor of **`ClickListener`** was called it pass its parameter's reference to its  instance variable the **`iOnClickEvent`** which has the data type of `IOnClickEvent`(our interface). It then calls the **`CallClick()`** which in turn calls the 
``` java 
iOnClickEvent.onClick();
```
and guess from  who's `onClick() `is that? It's from **Button** class . How? Remember this code? 

``` java
clickListener = new ClickListener(this);
```
we are passing the button itself.  We can think of t his as doing `button.onClick();`. 

I know, confusiiiiiiiiing! Here might help to visualize


<!-- <img src="{{site.baseurl}}/assets/img/blog/callback.png" alt="Girl in a jacket" style="width:400px;height:400px;">  -->


[![image_blog]({{site.baseurl}}/assets/img/blog/callback.png)]({{site.baseurl}}/assets/img/blog/callback.png)



### Android Example
I said from the start that I learned callbacks because I was using fragments. Well here is an example

``` java 
public class SampleFragment extends Fragment {

    public interface OnFragmentInteractionListener {
        // TODO: Update argument type and name
        void onCheck(boolean mark);
    }
    private OnFragmentInteractionListener mListener;
    boolean mark;
    Button button;
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {

        View view =  inflater.inflate(R.layout.fragment_sample, container, false);

        button = view.findViewById(R.id.click);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mark = !mark;
                mListener.onCheck(mark);
            }
        });
        return view;
    }
    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
        if (context instanceof OnFragmentInteractionListener) {
            mListener = (OnFragmentInteractionListener) context;   // this is where we attach the main activity to our mListener
        } else {
            throw new RuntimeException(context.toString()
                    + " must implement OnFragmentInteractionListener");
        }
    }
    @Override
    public void onDetach() {
        super.onDetach();
        mListener = null;
    }
}

// We implement the Interface
public class MainActivity extends AppCompatActivity implements SampleFragment.OnFragmentInteractionListener { 


    CheckBox checkBox;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        checkBox = findViewById(R.id.chc);
    }

    @Override
    public void onCheck(boolean mark) {
        checkBox.setChecked(mark);
    }
}

```

Output
<br />
![Alt Text](https://media.giphy.com/media/3o752e46d9Y7yP05Ko/giphy.gif)



### Conclusion
Callbacks is kinda hard to understand and it's a bit confusing. It takes time to get used to this pattern since it needs  you to change the way you think so don't stress yoursefl too much. That's it for today. **Remember to Smile, Code and Wave!**