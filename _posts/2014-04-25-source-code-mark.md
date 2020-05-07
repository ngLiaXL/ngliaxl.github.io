---
layout: post
title:  "Source Code"
date:   2014-04-25 11:53:27 +0800
categories: android
---

###  Remove duplicates from a list of objects based on property


```
public class Wrapper<T, U> {
    private T t;
    private Function<T, U> equalityFunction;

    public Wrapper(T t, Function<T, U> equalityFunction) {
        this.t = t;
        this.equalityFunction = equalityFunction;
    }

    public T unwrap() {
        return this.t;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        @SuppressWarnings("unchecked")
        Wrapper<T, U> that = (Wrapper<T, U>) o;
        return Objects.equals(equalityFunction.apply(this.t), that.equalityFunction.apply(that.t));
    }

    @Override
    public int hashCode() {
        return Objects.hash(equalityFunction.apply(this.t));
    }

    public interface Function<T, R> {
        R apply(T t);
    }

}
```





```
public static <T, U> void distinct(List<T> sourceList, Wrapper.Function<T, U> equalityFunction) {
        List<Wrapper<T, U>> wrapperList = new ArrayList<>();
        for (T t : sourceList) {
            wrapperList.add(new Wrapper<>(t, equalityFunction));
        }
        LinkedHashSet<Wrapper<T, U>> set = new LinkedHashSet<>(sourceList.size());

        set.addAll(wrapperList);
        wrapperList.clear();
        wrapperList.addAll(set);

        sourceList.clear();
        for (Wrapper<T, U> wrapper : wrapperList) {
            sourceList.add(wrapper.unwrap());
        }
    }
```

###  List多属性排序
```
public class MultiComparator<T> implements Comparator<T> {

    private final List<Comparator<T>> comparators;

    public MultiComparator(List<Comparator<T>> comparators) {
        this.comparators = comparators;
    }

    @SafeVarargs
    public MultiComparator(Comparator<T>... comparators) {
        this(Arrays.asList(comparators));
    }

    @Override
    public int compare(T o1, T o2) {
        for (Comparator<T> c : comparators) {
            int result = c.compare(o1, o2);
            if (result != 0) {
                return result;
            }
        }
        return 0;
    }

    @SafeVarargs
    public static <T> void sort(List<T> list, Comparator<T>... comparators) {
        Collections.sort(list, new MultiComparator<>(comparators));
    }
}
```

### 反射 Drawable
```
	private int getDrawableByReflect(String field) {
		int drawable = 0;
		// "cn.testreflect.R$drawable"
		String pkg = MainApplication.getContext().getPackageName();
		String clazz = pkg + ".R$drawable";
		try {
			Field f = Class.forName(clazz).getField(field);
			drawable = f.getInt(field);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return drawable;
	}
```
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

### MD5
```
public class MD5 {
    private static final String TAG = "MD5";
 
    public static boolean checkMD5(String md5, File updateFile) {
        if (TextUtils.isEmpty(md5) || updateFile == null) {
            Log.e(TAG, "MD5 string empty or updateFile null");
            return false;
        }
 
        String calculatedDigest = null;
  try {
   calculatedDigest = calculateMD5(updateFile);
  } catch (Exception e) {
   e.printStackTrace();
  }
  
        if (calculatedDigest == null) {
            Log.e(TAG, "calculatedDigest null");
            return false;
        }
 
        Log.v(TAG, "Calculated digest: " + calculatedDigest);
        Log.v(TAG, "Provided digest: " + md5);
 
        return calculatedDigest.equalsIgnoreCase(md5);
    }
 
    public static String calculateMD5(File updateFile) {
        MessageDigest digest;
        try {
            digest = MessageDigest.getInstance("MD5");
        } catch (NoSuchAlgorithmException e) {
            Log.e(TAG, "Exception while getting digest", e);
            return null;
        }
 
        InputStream is;
        try {
            is = new FileInputStream(updateFile);
        } catch (FileNotFoundException e) {
            Log.e(TAG, "Exception while getting FileInputStream", e);
            return null;
        }
 
        byte[] buffer = new byte[8192];
        int read;
        try {
            while ((read = is.read(buffer)) > 0) {
                digest.update(buffer, 0, read);
            }
            byte[] md5sum = digest.digest();
            BigInteger bigInt = new BigInteger(1, md5sum);
            String output = bigInt.toString(16);
            // Fill to 32 chars
            output = String.format("%32s", output).replace(' ', '0');
            return output;
        } catch (IOException e) {
            throw new RuntimeException("Unable to process file for MD5", e);
        } finally {
            try {
                is.close();
            } catch (IOException e) {
                Log.e(TAG, "Exception on closing MD5 input stream", e);
            }
        }
    }
}
```

