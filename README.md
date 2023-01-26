# commu-sidewalk

以R做平安走路許願帳戶的資料處理

[文件](https://docs.google.com/document/d/1bOuwkF_0abTdAyhnxUxhxrwExjNR9BnhfFtT7l5Vzc4/edit?usp=sharing)

## Develop

檔案架構：
```text
│  .gitignore
│  commu-sidewalk.Rproj # RStudio project file
├─data                  # 儲存資料
├─output                # 輸出資料
└─src                   # R scripts
        1_download.R
        2_cleaning.R
```

### Library Used

- jsonlite

### 1\_download.R

從commutag下載標註圖片`df_img`及資料集資訊`lst_info`（in `/data/raw.RData`）。

會把`df_img`存成`/data/今日日期.csv`

改變`is_limit`可以下載有限的或整組圖片資料。

### 2\_cleaning.R

- [ ] 從`lst_info`取得form把`df_img`的formReply醜id替換掉。
