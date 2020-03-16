---
layout: post
title:  "Source Code"
date:   2014-04-25 11:53:27 +0800
categories: android
---


### 位运算运用-权限
```
public class Permission {

    public static final int READ = 1 << 0; // 0001

    public static final int WRITE = 1 << 1; // 0010

    private int permission;

    /**
     * 设置权限
     */
    public void set(int permission) {
        this.permission = permission;
    }

    /**
     * 添加权限
     */
    public void add(int permission) {
        this.permission |= permission;
    }

    /**
     * 删除权限
     */
    public void cancel(int permission) {
        this.permission &= ~permission;
    }

    /**
     * 是否有此权限
     */
    public boolean allow(int permission) {
        return (this.permission & permission) == permission;
    }

    /**
     * 是否没有此权限
     */
    public boolean notAllow(int permission) {
        return (this.permission & permission) == 0;
    }

    /**
     * 是否只拥有此权限
     */
    public boolean onlyAllow(int permission) {
        return this.permission == permission;
    }


}
```

### Assets copy
```
    private void copyAssets() {
        AssetManager assetManager = getAssets();
        String[] files = null;
        try {
            files = assetManager.list("");
        } catch (IOException e) {
            Log.e("tag", "Failed to get asset file list.", e);
        }
        if (files != null) for (String filename : files) {
            InputStream in = null;
            OutputStream out = null;
            try {
                in = assetManager.open(filename);
                File outFile = new File(getExternalFilesDir(null), filename);
                out = new FileOutputStream(outFile);
                copyFile(in, out);
            } catch(IOException e) {
                Log.e("tag", "Failed to copy asset file: " + filename, e);
            }
            finally {
                if (in != null) {
                    try {
                        in.close();
                    } catch (IOException e) {
                        // NOOP
                    }
                }
                if (out != null) {
                    try {
                        out.close();
                    } catch (IOException e) {
                        // NOOP
                    }
                }
            }
        }
    }
    private void copyFile(InputStream in, OutputStream out) throws IOException {
        byte[] buffer = new byte[1024];
        int read;
        while((read = in.read(buffer)) != -1){
            out.write(buffer, 0, read);
        }
    }
```

### Get the process name
```
public static String getProcessName(Context cxt, int pid) {
    ActivityManager am = (ActivityManager) cxt.getSystemService(Context.ACTIVITY_SERVICE);
    List<ActivityManager.RunningAppProcessInfo> runningApps = null;
    if (am != null) {
        runningApps = am.getRunningAppProcesses();
    }
    if (runningApps == null) {
        return null;
    }
    for (ActivityManager.RunningAppProcessInfo processInfo : runningApps) {
        if (processInfo.pid == pid) {
            return processInfo.processName;
        }
    }
    return null;
}

```

```
public static String getProcessName() {
  try {
    File file = new File("/proc/" + android.os.Process.myPid() + "/" + "cmdline");
    BufferedReader mBufferedReader = new BufferedReader(new FileReader(file));
    String processName = mBufferedReader.readLine().trim();
    mBufferedReader.close();
    return processName;
  } catch (Exception e) {
    e.printStackTrace();
    return null;
  }
}
```

### Copy File
```
    public static void copy(File src, File dst) throws IOException {
        InputStream in = new FileInputStream(src);
        try {
            OutputStream out = new FileOutputStream(dst);
            try {
                // Transfer bytes from in to out
                byte[] buf = new byte[1024];
                int len;
                while ((len = in.read(buf)) > 0) {
                    out.write(buf, 0, len);
                }
            } finally {
                out.close();
            }
        } finally {
            in.close();
        }
    }
```


