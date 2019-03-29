# geocode
百度BD09、高德GCJ02、谷歌WGS84坐标系互转

转换基础服务由https://tool.lu/coordinate/ 提供。

此代码是基于该网站的服务，提供一个用R操作的、批量化的坐标转换解决方案。


## 1.安装必须的程序包
```R
install.packages("jsonlite")
install.packages("httr")
install.packages("readxl")
```


## 2. 定义三种常见坐标系
```R
geotype1<- "wgs84"
geotype2<- "gcj02"
geotype3<- "bd09"
```

## 3. Post请求数据
```R
bdc<- paste("src_type=",geotype3,"&src_coordinate=",toString(114.1861251),",",toString(22.29358599),sep = "", collapse = NULL)
## 上行代码中，geotype3可替换为其他2种。

req <- httr::POST("https://tool.lu/coordinate/ajax.html",
                  httr::add_headers(
                    "Content-Type" = "application/x-www-form-urlencoded;charset=UTF-8"
                  ),
                  body = bdc
);

json <- httr::content(req, as = "text")

output <- fromJSON(json)
```

## 4. 获取数据
```R
wgs84_lat<- output$result$wgs84$lat
wgs84_lng<- output$result$wgs84$lng

gc02_lat<- output$result$gcj02$lat
gc02_lng<- output$result$gcj02$lng

bd09_lat<- output$result$bd09$lat
bd09_lng<- output$result$bd09$lng
```

## 5. 说明
第四步和第五步可以合并，放入到循环结构。且在第2步之前可以预先读入含有坐标的excel文件，存为dataframe。每次循环结构存储一次数据，最后统一导出结果。
