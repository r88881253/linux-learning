# linux-learning

## 常見指令

### sed - stream editor 字串處理

- `-n`：--quiet, --silent，沉默模式。
- `-e`：script, --expression=script，直接在命令模式編輯。(可不加，請見詳解)
- `-f`：script-file, --file=script-file，程式手稿不直接在命列中打上，而是從指定的檔案中載入。
- `-i`：-i[SUFFIX], --in-place[=SUFFIX]，修改檔案。

#### sed -n
1. 搜尋 sed -n '/pattern/=' filename，-n 選項只列印行號，不列印檔案內容
    ```console
    // 使用grep -n 的效果
    [MacBook-Pro:...]$ grep -n running sed_example.txt 
    11:8. "The pain of running relieves the pain of living." – Jacqueline Simon Gunn
    13:9. "If you run, you are a runner. It doesn’t matter how fast or how far. It doesn’t matter if today is your first day or if you’ve been running for twenty years. There is no test to pass, no license to earn, no membership card to get. You just run." – John Bingham
   
    // 使用sed -n 的效果
   [MacBook-Pro:...]$ sed -n '/running/=' test.txt 
    11
    13
   
    // 「-n」 有使用取代的須加「p」才會顯示行數
    [MacBook-Pro:...]$ sed -n 's/running/------/p' test.txt 
    8. "The pain of ------ relieves the pain of living." – Jacqueline Simon Gunn
    9. "If you run, you are a runner. It doesn’t matter how fast or how far. It doesn’t matter if today is your first day or if you’ve been ------ for twenty years. There is no test to pass, no license to earn, no membership card to get. You just run." – John Bingham
   
    // 「-n」不加「p」就不會有output
    [MacBook-Pro:...]$ sed -n 's/running/------/' sed_example.txt 
    ```

#### 常用操作

1. 查看幾行到幾行
    ```console
    sed -n '1,5p' sed_example.txt
    ```
    


### vi - 文書處理器
#### 常用操作
<pre>
0=單行首 
$=單行末

gg=文件首行  
G=文件末行  
</pre>

### find - 檔案搜尋

- `-type`：
   - `f`: 一般檔案
   - `d`: 目錄
- `-name`: 檔名
- `-user`: 檔案擁有者
- `時間參數`: 
   - `-mtime n`: 檔案的最後修改時間（modification time），單位為天
   - `-atime n`:檔案的最後存取時間（access time），單位為天
   - `-cmin n`: 檔案狀態相關資訊最後修改的時間（status time），單位為分鐘


常用操作

1. 查詢某個時間內檔案
   - n天內修改的(-ctime)
      ```
      # -c 是change的意思
      # 查詢1天內改過的file，並列出相關資訊
      find . -type f -ctime -1 | xargs ls –l
      # 查詢30分鐘內內改過的檔案，並列出相關資訊
      find . -type f -cmin -1 | xargs ls –l
      ```
   - n天內訪問過的(-atime) 
      ```
      # 查詢1天內存取過的檔案
      find . -type f -atime -1 | xargs ls -l
      ```

### watch - 重複執行執行linux指令

預設為每2秒執行一次
範例：

1. 每2秒
    ```console
    watch free -m
    ```
    
2. 改為每10秒執行script.sh
    ```console
    watch -n 10 script.sh
    ```
