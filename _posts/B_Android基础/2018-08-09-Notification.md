---
layout: post
title: Notification
category: Android基础
tags: Essay
keywords: 
---

### 1.如何使用通知

注意Android 8以上使用通知需要考虑通道

```
NotificationManager manager = (NotificationManager)getSystemService(Context.NOTIFICATION_SERVICE);


NotificationCompat.Builder builder = 
new NotificationCompat.Builder(MainActivity.this)
                        .setContentTitle("通知标题")
                        .setContentText("通知文本")
                        .setWhen(System.currentTimeMillis())
                        .setSmallIcon(R.drawable.ic_launcher_background)                .setLargeIcon(BitmapFactory.decodeResource(getResources(),R.drawable.ic_launcher_background));
                        
//在这里判断系统版本是否大于8.0，大于8.0则创建一个Channel
if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O){

NotificationChannel channel = new NotificationChannel(CHANNEL_ID,"my_channel",NotificationManager.IMPORTANCE_DEFAULT);
channel.enableLights(true); //是否在桌面icon右上角展示小红点
channel.setLightColor(Color.GREEN); //小红点颜色
channel.setShowBadge(true); //是否在久按桌面图标时显示此渠道的通知
mNotificationManager.createNotificationChannel(channel);//将manager与channel绑定
builder.setChannelId("0")//这里将通知与刚才的channel id绑定

}
                        
Notification notification = builder.build();//构建通知对象             
manager.notify(1,notification);//利用manager发送通知

}
```



### 2.悬挂式通知

其实悬挂式通知跟普通的只有一行代码的区别：

```
Notification.Builder builder = new Notification.Builder(MainActivity.this);

Intent intent = new Intent(MainActivity.this, SecondActivity.class);
PendingIntent pendingIntent = PendingIntent.getActivity(MainActivity.this, 0, intent, 0);

builder.setSmallIcon(R.mipmap.ic_launcher)
.setContentIntent(pendingIntent)
.setContentText("contenttext")
.setContentTitle("contenttitle")
.setSubText("subtext")
.setAutoCancel(false)
.setFullScreenIntent(pendingIntent, true) //这行是关键
.setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher));
                
Notification notification = builder.build();
                
NotificationManager notificationManager = (NotificationManager)getSystemService(NOTIFICATION_SERVICE);
                
notificationManager.notify(0, notification);
```



### 3.自定义通知



#### 第一步:准备布局文件

这个布局文件有特殊要求：
仅支持FrameLayout、LinearLayout、RelativeLayout三种布局控件
AnalogClock、Chronometer、Button、ImageButton、ImageView、ProgressBar、TextView、ViewFlipper、ListView、GridView、StackView和AdapterViewFlipper这些显示控件
使用其他View会引起ClassNotFoundException异常。

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
 
    >
    <ImageView
        android:id="@+id/title_icon"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:layout_marginLeft="16dp"
        android:layout_marginTop="8dp"
        android:layout_marginBottom="8dp"
        android:src="@drawable/title_icon"
        android:layout_marginStart="16dp" />
    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="100dp"
        android:layout_toRightOf="@+id/title_icon"
        android:layout_marginRight="16dp"
        android:layout_marginTop="8dp"
        android:layout_marginBottom="8dp"
        android:layout_marginEnd="16dp"
        android:layout_toEndOf="@+id/title_icon">
        <TextView
            android:id="@+id/song_title"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="七里香"
            android:textSize="20sp"
            android:textStyle="bold"
            android:layout_marginLeft="16dp"
            android:layout_marginTop="16dp"
            android:layout_marginStart="16dp" />
        <TextView
            android:id="@+id/singer"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="周杰伦"
            android:layout_marginLeft="16dp"
            android:layout_marginTop="4dp"
            android:layout_below="@+id/song_title"
            android:layout_marginStart="16dp" />
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_below="@id/singer"
            android:weightSum="5"
            android:orientation="horizontal"
            >
            <Button
                android:id="@+id/love"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="@string/song_like"
                android:textColor="@android:color/holo_red_light"
                android:background="@android:color/transparent"
                />
            <Button
                android:id="@+id/song_back"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="@string/song_back"
                android:textSize="16sp"
                android:textColor="@android:color/black"
                android:background="@android:color/transparent"
                />
            <Button
                android:id="@+id/song_start"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="||"
                android:textColor="@android:color/black"
                android:background="@android:color/transparent"
                />
            <Button
                android:id="@+id/song_front"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text=">"
                android:textSize="16sp"
                android:textColor="@android:color/black"
                android:background="@android:color/transparent"
            />
            <Button
                android:id="@+id/song_font"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="词"
                android:textColor="@android:color/darker_gray"
                android:background="@android:color/transparent"
                />
        </LinearLayout>
        <TextView
            android:id="@+id/btn_item_close"
            android:layout_width="20dp"
            android:layout_height="20dp"
            android:layout_alignParentEnd="true"
            android:layout_alignParentTop="true"
            android:layout_alignParentRight="true"
            android:gravity="center"
            android:text="X"
            android:textStyle="bold"
            />
 
    </RelativeLayout>
 
</RelativeLayout>
```





#### 第二步:继承RemoteViews自定义

```
public class MyNotificationView extends RemoteViews implements View.OnClickListener {
    private RelativeLayout mLayout;
    private Activity mActivity;
    private TextView mTvClose;
    private LayoutInflater mLayoutInflater;

    public MyNotificationView(String packageName, int layoutId, Activity activity) {
        super(packageName, layoutId);
        mActivity=activity;
        mLayoutInflater=activity.getLayoutInflater();
        mLayout= (RelativeLayout) mLayoutInflater.inflate(layoutId,null);
        mTvClose=mLayout.findViewById(R.id.btn_item_close);
        mTvClose.setOnClickListener(this);
    }

    //无法监听
    @Override
    public void onClick(View v) {
        switch (v.getId()){
            case R.id.btn_item_close:
                Toast.makeText(mActivity,"关闭成功",Toast.LENGTH_SHORT).show();
                Log.d("abcd", "onClick: ");
                break;
        }
    }
}
```



#### 第三步：使用自定义发送通知

添加通知View，添加点击事件，添加一个Notification

```
Notification.Builder builder=new Notification.Builder(activity);
                Intent in=new Intent("close");
                in.setPackage(activity.getPackageName());
                PendingIntent intent=PendingIntent.getBroadcast(activity,0
                        ,in,PendingIntent.FLAG_UPDATE_CURRENT);
 
                MyNotificationView myNotificationView=new MyNotificationView(activity.getPackageName()
                        ,R.layout.notification_item
                        ,activity);
                myNotificationView.setOnClickPendingIntent(R.id.btn_item_close,intent);
 
                builder.setSmallIcon(R.mipmap.ic_launcher)
                        .setOngoing(false)
                        .setAutoCancel(true)
                        .setCustomContentView(myNotificationView)
                        .setChannelId(CHANNEL_ID)
                        .setWhen(SystemClock.currentThreadTimeMillis())
                        .setContentIntent(intent);
                mNotificationManager.notify(mNotificationId++, builder.build());
```

