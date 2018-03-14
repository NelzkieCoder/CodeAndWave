---
title: Android - Repeating Alarm
layout: post
tagline: For android scheduling and notification
image: https://postimg.org/image/vefxherup/
feature-img: assets/img/unsplash/coding.jpg
tags:
- AlarmManager
- Anroid

keywords: Android, AlarmManager, Alarm
lang: en
category: articles
publisher: CodeAndWave
---

There are two ways to set a repeating alarm on android. One involves calling the `setRepeating()` and the other involves manually resetting the alarm after it was fired up


## Automatic way

To set a repeating alarm easily we use the setRepeating() on android

```java
Intent myIntent = new Intent(this,AlarmReceiver.class);
        PendingIntent pendingIntent = PendingIntent.getBroadcast(this,0,myIntent,PendingIntent.FLAG_UPDATE_CURRENT);
        Calendar calendar = Calendar.getInstance();
        calendar.setTimeInMillis(System.currentTimeMillis());
        calendar.set(Calendar.SECOND, 10);
        alarmManager.setRepeating(AlarmManager.RTC_WAKEUP,calendar.getTimeInMillis(),60000,pendingIntent);
```

see the **60000** argument pass there? Before kitkat you can set that to what ever milliseconds you like, but on kitkat onwards, the minimum is 1 min (60000 milliseconds). This is due to battery saving feature introduce on kitkat version.

``` java
 setRepeating (int type, 
                long triggerAtMillis, 
                long intervalMillis, 
                PendingIntent operation)
```

`int type` -  the type of alarm

` long triggerAtMillis `- this is the time on when the alarm would fire up

`long intervalMillis `- the interval time , can be  	**INTERVAL_DAY**,  **INTERVAL_HALF_DAY**,  **INTERVAL_HOUR**, **INTERVAL_HALF_HOUR**, **INTERVAL_FIFTEEN_MINUTES**

`PendingIntent operation `- the pending intent

On our interval millis, we set it to 1 minute, but if you want a daily alarm set it to **INTERVAL_DAY**

The alarm would always fire every *1 minute and 10 seconds*.

### Not so easy after all

If you want to have an alarm daily, the solution above is perfect but if you are developing say for example an alarm app, you got to have a flexible alarm manager. What if on the alarm app for example we set the alarm to fire up weekdays mon-fri? Obviously our code above if set to INTERVAL_DAY will kept on firing each day no matter what day it is. To remedy the problem what we can do is cancel the alarm when the receiver was called. But before we we the code, we need to know that in order to cancel an alarm, we need to make sure that the pending intent is the same as the one we declare when setting an alarm. Same like this,

`PendingIntent pendingIntent = PendingIntent.getBroadcast(this,0,myIntent,PendingIntent.FLAG_UPDATE_CURRENT);`

##### To cancel an alarm

``` java
@Override
    public void onReceive(Context context, Intent intent) {
        alarmManager = (AlarmManager) context.getSystemService(Context.ALARM_SERVICE);
        Toast.makeText(context, "Repeating alarm", Toast.LENGTH_SHORT).show();
        Calendar calendar = Calendar.getInstance();
        calendar.setTimeInMillis(System.currentTimeMillis());
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("hh:mm:ss");
        String dateFormat = simpleDateFormat.format(calendar.getTime());
        NotificationFire(context, dateFormat);

        if (calendar.get(Calendar.DAY_OF_WEEK) == Calendar.FRIDAY) {
            Toast.makeText(context, "cancel called", Toast.LENGTH_SHORT).show();
            Intent myIntent = new Intent(context,AlarmReceiver.class);
            PendingIntent pendingIntent = PendingIntent.getBroadcast(context,0,myIntent,PendingIntent.FLAG_UPDATE_CURRENT);
            pendingIntent.cancel();
            alarmManager.cancel(pendingIntent);
        }
    }
```

When the notification fired up on friday, we would cancel our alarm manager so it wont fire the notification on weekends.





### The manual way

On our manual we, we don't use the `setRepeating() ` instead we use the `setExact()` which can only be used once hence we have to reset it on our **Receiver** class

**Main Acitivty**


``` java
Intent myIntent = new Intent(this,AlarmReceiverHard.class);
        PendingIntent pendingIntent = PendingIntent.getBroadcast(this,0,myIntent,PendingIntent.FLAG_UPDATE_CURRENT);
        Calendar calendar = Calendar.getInstance();
        calendar.setTimeInMillis(System.currentTimeMillis());
        calendar.set(Calendar.SECOND, 10);
        alarmManager.setExact(AlarmManager.RTC_WAKEUP, calendar.getTimeInMillis(), pendingIntent);
```

Set the alarm to fire up in 10 seconds

**Receiver class**

``` java
 AlarmManager alarmManager;
    @Override
    public void onReceive(Context context, Intent intent) {

        alarmManager = (AlarmManager) context.getSystemService(Context.ALARM_SERVICE);
Intent myIntent = new Intent(context,AlarmReceiverHard.class);
        PendingIntent pendingIntent = PendingIntent.getBroadcast(context,1,myIntent,PendingIntent.FLAG_UPDATE_CURRENT);
        Calendar calendar = Calendar.getInstance();
        calendar.add(Calendar.DAY_OF_MONTH, 1);
        //calendar.setTimeInMillis(System.currentTimeMillis());
        calendar.set(Calendar.SECOND, 10);


        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("hh:mm:ss");
        String dateFormat = simpleDateFormat.format(calendar.getTime());
        NotificationFire(context, " hahahaha  "+dateFormat);
    }
```

When the receiver was called, we reset the alarm manager manually. To make it flexible say for example we want to alarm on Weekdays we just need to check if the current day is part of weekdays like this

``` java
if(calendar.get(Calendar.DAY_OF_WEEK) != Calendar.SATURDAY || calendar.get(Calendar.DAY_OF_WEEK) != Calendar.SUNDAY){
alarmManager.setExact(AlarmManager.RTC_WAKEUP, calendar.getTimeInMillis(), pendingIntent);
}
```


#### The PROBLEM!

Notice something on our solutions? On the first solution since we canceled it on weekends, there is no way of setting it back once monday comes. Same can be said to our second solution, since we are setting it manually when weekend comes we just stop resetting it but once weekdays starts there is no way of setting it back. To remedy this, the solution i could think of is that we need to have **another alarm manager that would keep on firing every day** to check if it is weekdays or weekend and then the daily alarm manager appropriately.


### Doze Mode

Beginning in android Marshmallow, android has a new feature called doze mode. This changes the behavior of the alarm manager and will surely mess up the waking time of the phone when it enters doze mode. To remedy this, we just use the `setAndAllowWhileIdle()` or the `setAlarmclock() `

``` java
alarmManager.setAndAllowWhileIdle(AlarmManager.RTC_WAKEUP, d.toDate().getTime(), pendingIntent);
```


### Conclusion

Dealing with alarm and date and time can be a pain. Instead of doing this manually I would suggest using a 3rd party library:

**Jodatime for android** - For Date and Time management 

**Evernote job** - For android scheduling