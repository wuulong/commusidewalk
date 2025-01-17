# commu-sidewalk

以R做平安走路許願帳戶的資料處理

- [補充文件](https://docs.google.com/document/d/1bOuwkF_0abTdAyhnxUxhxrwExjNR9BnhfFtT7l5Vzc4/edit?usp=sharing)
- [Google試算表  v20230127](https://docs.google.com/spreadsheets/d/17eUoIsEBugXniR0klzlvmXeewM029A7fDpOw2HI6MSY/edit#gid=558584087)

## Develop

檔案架構：
```text
│  .gitignore
│  commusidewalk.Rproj # RStudio project 檔案
│  main.R
│  README.md
│
├─data # 儲存各階段.RData
│
├─output
│      20230130.csv
│      20230130_rank.csv
│
├─R
│      1_download.R  # 從commutag下載最新資料
│      2_cleaning.R  # 資料清洗與欄位重新命名排序
│      3_rank.R      # 計算分數
│      4_output.R    # 畫圖
│      rank_lookup.R # 3_rank.R 用到的lookuptable
│      testtest.R    # 測試script用
│
└─tests
    │  testthat.R
    │
    └─testthat
            test-3_rank.R # 測試評分演算法
```

## main.R 自動化

Linux
```sh
# cd to project directory (same floor with .gitignore)
> Rscript main.R
```

Windows
```powershell
> Rscript.exe main.R
```

```sh
> Rscript main.R
> Select action and type Enter:
[1] create new csv from commutag platform. # 跑`/src`底下所有數字編號開頭檔案，匯出資料到`/output`，檔名是`日期.csv`
[2] convert shortform to chinese.          # shortform.csv to shortform_chinese.csv
[3] convert chinese to shortform.          # 同上，功能相反

Your selection: 2
Please provide full file location (or just type Enter if your file is called `./short
form.csv`): ......
```

## 關於標頭

read.csv之後發現一些符號如`原始總寬度－是否夠寬讓輪椅通行`會被轉成`原始總寬度.是否夠寬讓輪椅通行`（`佔用情形(多選)-導致兩人交會需停讓或淨寬<1.5米` -> `佔用情形.多選..導致兩人交會需停讓或淨寬.1.5米`），導致shortform和chinese的轉換會失敗，因此現在移除了特殊字元，對照表在`main.R`。

## Library Used

- jsonlite
- tidyverse
- ggplot2

## 檔案描述

### 1\_download.R

從commutag下載標註圖片`df_img`及資料集資訊`lst_info`（in `/data/raw.RData`）。

改變`is_limit`可改為下載有限的(8筆)或整組圖片資料。

### 2\_cleaning.R

1. rename formReply id (ugly) to chinese question name
2. use `|` as deliminator for list (we cannot save list in csv)
3. formReply$value -> formReply
4. 加入哈爸資料有的我這邊沒有的欄位，設成NA
5. 重新命名欄位
6. 重新排序成可以餵給App script的順序
7. 儲存成csv（Excel會亂碼，但LibreOffice可以正常開）

### 3\_output.R

試玩ggplot2，產生照片數量的時間序列圖。

### testtest.R

試跑code用。