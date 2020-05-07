---
layout: post
title:  "[Android]	Is App In Background"
date:   2017-01-24 11:53:27 +0800
categories: android
---
```
    public static boolean isInBackground(Context context) {

        boolean isInBackground = true;

        ActivityManager am = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);

        List taskInfo;

        if (VERSION.SDK_INT > 20) {

            taskInfo = am.getRunningAppProcesses();

            Iterator componentInfo = taskInfo.iterator();
 
 
            while (true) {<br>
                RunningAppProcessInfo processInfo;
                do {
                    if (!componentInfo.hasNext()) {
                        return isInBackground;
                    }
 
 
                    processInfo = (RunningAppProcessInfo) componentInfo.next();
                } while (processInfo.importance != 100);
 
 
                String[] pkgs = processInfo.pkgList;
                int length = pkgs.length;
 
 
                for (int i = 0; i < length; ++i) {
                    String activeProcess = pkgs[i];
                    if (activeProcess.equals(context.getPackageName())) {
                        return false;
                    }
                }
            }
        } else {
            taskInfo = am.getRunningTasks(1);
            ComponentName var10 = ((RunningTaskInfo) taskInfo.get(0)).topActivity;
            if (var10.getPackageName().equals(context.getPackageName())) {
                isInBackground = false;
            }
        }
 
 
        return isInBackground;
 
    }

 
 
 
 
 
 
    public static boolean isAppRunning(Context context, String name) {
        if (TextUtils.isEmpty(name)) {
            return false;
        } else {
            ActivityManager am = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
            List infos = am.getRunningAppProcesses();
            if (infos == null) {
                return false;
            } else {
                Iterator iterator = infos.iterator();
 
 
                RunningAppProcessInfo info;
                do {
                    if (!iterator.hasNext()) {
                        return false;
                    }
 
 
                    info = (RunningAppProcessInfo) iterator.next();
                } while (!info.processName.equals(name));
 
 
                return true;
            }
        }
    }
```

 






