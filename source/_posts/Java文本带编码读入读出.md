---
title: Java文本带编码读入读出
date: 2016-11-25 10:23:32
tags: "Tools"
---


很好用的java文件输入输出，不过内容比较简单，实际需要的时候还需要加上个性化设计。有空的时候可以更加模块化这个东西，主要是红来复制粘贴的...
```Java
 public static void write(String path, String content)  
            throws IOException {  
        File file = new File(path);  
        file.delete();  
        file.createNewFile();  
        BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(  
                new FileOutputStream(file), "UTF-8"));  
        writer.write(content);  
        writer.close();  
    }  
  
    public static String read(String path) throws IOException {  
        String content = "";  
        File file = new File(path);  
        BufferedReader reader = new BufferedReader(new InputStreamReader(  
                new FileInputStream(file), "GBK"));  
        String line = null;  
        while ((line = reader.readLine()) != null) {  
            content += line + "\n";  
        }  
        reader.close();  
        content = content.replace("\n", "")
        		.replace("<&nbsp>", "")
        		.replaceAll("<p>", "<p>&nbsp");
        //System.out.println(content);
        return content;  
    }
```