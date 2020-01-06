# 簡要說明每個 Rscript 的功用以及產出之資料的內容

## 匯入及製作1-1500行資料的df
```{r}
fps1 <- list.files("GS66/15", full.names = T)
# Initialize jiebaR and the dictionary
seg1<-worker(user="keywords.txt",stop_word = "stopwords.txt")

# 空字串
contents1 <- vector("character", length(fps1))
for (i in seq_along(fps1)) {
  # Read post from file
  post1 <- readLines(fps1[i], encoding = "UTF-8")
  # Segment post
  segged1 <- segment(post1, seg1)
  contents1[i] <- paste(segged1, collapse = " ")
}
# 製作df
to1500 <- tibble::tibble(id = seq_along(contents1), content = contents1)
```
## 斷詞
```{r}
#詞頻
to1500<-to1500 %>%
  unnest_tokens(output="word",
                input="content", 
                token="regex",
                pattern = " ")  

#計算出現詞彙次數
topicword_1500<-to1500 %>% 
  group_by(word) %>%
  summarise(n = n()) %>%
  arrange(desc(n))
```
## 製作年份最新二字+文字雲
```{r}
wordcloud2(topicword_1500, color = "random-light",fontFamily = "微軟正黑體", backgroundColor = "black")
```

## 以同樣斷詞方法製作四字+文字雲
```{r}
to1500_idiom<-to1500 %>% filter(str_detect(to1500$word, ".{4}"))

#計算出現詞彙次數
topicword_1500_idiom<-to1500_idiom %>% 
  group_by(word) %>%
  summarise(n = n()) %>%
  arrange(desc(n))

#製作年份最新四字文字雲
wordcloud2(topicword_1500_idiom, color = "random-light",fontFamily = "微軟正黑體", backgroundColor = "black")
```
### 以同樣匯入資料、製作df與斷詞方法製作次新、次舊、最舊文字雲
```{r}
#3001to4500行
fps3 <- list.files("GS66/45", full.names = T)
# Initialize jiebaR and the dictionary
seg3<-worker(user="keywords.txt",stop_word = "stopwords.txt")

# Initialize empty vector to use in for loop
contents3 <- vector("character", length(fps3))

for (i in seq_along(fps3)) {
  # Read post from file
  post3 <- readLines(fps3[i], encoding = "UTF-8")
  
  # Segment post
  segged3 <- segment(post3, seg3)
  contents3[i] <- paste(segged3, collapse = " ")
}

# Combine results into a df
to6000 <- tibble::tibble(id = seq_along(contents3), content = contents3)

#詞頻
to6000<-to6000 %>%
  unnest_tokens(output="word", 
                input="content", 
                token="regex",
                pattern = " ") 

#計算出現詞彙次數
topicword_6000<-to6000 %>% 
  group_by(word) %>%
  summarise(n = n()) %>%
  arrange(desc(n))

#製作年份次舊二字文字雲
wordcloud2(topicword_6000, color = "random-light",fontFamily = "微軟正黑體", backgroundColor = "black")
```

```{r}
to6000_idiom<-to6000 %>% filter(str_detect(to6000$word, ".{4}"))

#計算出現詞彙次數
topicword_6000_idiom<-to6000_idiom %>% 
  group_by(word) %>%
  summarise(n = n()) %>%
  arrange(desc(n))

#製作年份次舊四字文字雲
wordcloud2(topicword_6000_idiom, color = "random-light",fontFamily = "微軟正黑體", backgroundColor = "black")
```

```{r}
#to end
fps4 <- list.files("GS66/end", full.names = T)
# Initialize jiebaR and the dictionary
seg4<-worker(user="keywords.txt",stop_word = "stopwords.txt")

# Initialize empty vector to use in for loop
contents4 <- vector("character", length(fps4))

for (i in seq_along(fps4)) {
  # Read post from file
  post4 <- readLines(fps4[i], encoding = "UTF-8")
  
  # Segment post
  segged4 <- segment(post4, seg4)
  contents4[i] <- paste(segged4, collapse = " ")
}

# Combine results into a df
end<- tibble::tibble(id = seq_along(contents4), content = contents4)

#詞頻
end<-end %>%
  unnest_tokens(output="word",  
                input="content",  
                token="regex",
                pattern = " ")

#計算出現詞彙次數
topicword_end<-end %>% 
  group_by(word) %>%
  summarise(n = n()) %>%
  arrange(desc(n))

#製作年份最舊二字文字雲
wordcloud2(topicword_end, color = "random-light",fontFamily = "微軟正黑體", backgroundColor = "black")
```

```{r}
end_idiom<-end%>% filter(str_detect(end$word, ".{4}"))

#計算出現詞彙次數
topicword_end_idiom<-end_idiom %>% 
  group_by(word) %>%
  summarise(n = n()) %>%
  arrange(desc(n))

#製作年份最舊四字文字雲
wordcloud2(topicword_end_idiom, color = "random-light",fontFamily = "微軟正黑體", backgroundColor = "black")
```

## 以同樣匯入資料方法製作臺灣外交部df
```{r}
fps01 <- list.files("TW", full.names = T)
# Initialize jiebaR and the dictionary
seg01<-worker(user="keywordsm.txt",stop_word = "stopwordsm.txt")

# Initialize empty vector to use in for loop
contents01 <- vector("character", length(fps01))

for (i in seq_along(fps01)) {
  # Read post from file
  post01 <- readLines(fps01[i], encoding = "UTF-8")
  
  # Segment post
  segged01 <- segment(post01, seg01)
  contents01[i] <- paste(segged01, collapse = " ")
}

# Combine results into a df
docs_df01 <- tibble::tibble(id = seq_along(contents01), content = contents01)
```

### 製作臺灣外交部二字+文字雲
```{r}
#詞頻
taiwan<-docs_df01 %>%
  unnest_tokens(output="word",  
                input="content",  
                token="regex",
                pattern = " ")

taiwan2<-taiwan %>% filter(str_detect(taiwan$word, ".{2}"))

#計算出現詞彙次數
topicword_taiwan2<-taiwan2 %>% 
  group_by(word) %>%
  summarise(n = n()) %>%
  arrange(desc(n))

#製作文字雲
wordcloud2(topicword_taiwan2, color = "random-light",fontFamily = "微軟正黑體", backgroundColor = "black")
```

### 製作臺灣外交部四字+文字雲
```{r}
taiwan4<-taiwan %>% filter(str_detect(taiwan$word, ".{4}+"))

#計算出現詞彙次數
topicword_taiwan4<-taiwan4 %>% 
  group_by(word) %>%
  summarise(n = n()) %>%
  arrange(desc(n))

#製作文字雲
wordcloud2(topicword_taiwan4, color = "random-light",fontFamily = "微軟正黑體", backgroundColor = "black")
```
