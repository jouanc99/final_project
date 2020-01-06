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

