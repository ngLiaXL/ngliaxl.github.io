---
layout:	post
title:	"[Android]	Scheduled handler thread"
date:	2022-07-12
---


```
public class ScheduledHandlerThread {

    private HandlerThread mHandlerThread;
    private Handler mHandler;

    private String mName;

    public ScheduledHandlerThread(String name) {
        this.mName = name;
        initIfNeeded();
    }

    private void initIfNeeded() {
        if (mHandlerThread == null) {
            mHandlerThread = new HandlerThread(mName);
            mHandlerThread.start();
            mHandler = new Handler(mHandlerThread.getLooper());
        }
    }


    public void postDelayed(Runnable task, long delaySeconds) {
        initIfNeeded();
        mHandler.postDelayed(task, delaySeconds * 1000);
    }

    public void postDelayedWithRemoveAll(Runnable task, long delaySeconds) {
        initIfNeeded();
        mHandler.removeCallbacksAndMessages(null);
        mHandler.postDelayed(task, delaySeconds * 1000);
    }

    public void removeCallbacksAndMessages(){
        mHandler.removeCallbacksAndMessages(null);
    }

    public void quit() {
        if (mHandlerThread != null) {
            mHandlerThread.quit();
            mHandlerThread = null;
        }
    }
}

```



	

