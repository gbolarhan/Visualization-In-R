# Business Reports with R

This project analyzes business report data using R. It includes text processing, word cloud generation, TF-IDF analysis, and bigram analysis.

## Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/yourusername/business-reports-with-r.git
   ```
2. Navigate to the project directory:
   ```sh
   cd business-reports-with-r
   ```
3. Install the required R packages:
   ```r
   install.packages(c("tidyverse", "tidytext", "wordcloud", "RColorBrewer"))
   ```

## Usage

1. Open the project in RStudio or your preferred R environment.
2. Run the R script or R Markdown file to perform the analysis:
   ```r
   source("A2Assignment.Rmd")
   ```

## Code Explanation

### Word Cloud Generation

```r
    wordcloud(threads_words_count$word, threads_words_count$n,
    max.words = 50,
    colors = brewer.pal(3, "Dark2"), scale = c(1, 1)
    )
```

Generates a word cloud from the top 50 words in the dataset.

# Data Processing

```r
threads_tbl <- as_tibble(threads_df) %>%
  unite(title, text, text_comments, col = "text", sep = " ")
threads_words <- threads_tbl %>%
  unnest_tokens(word, text) %>%
  group_by(word) %>%
  filter(n() > 10) %>%
  ungroup()
threads_words_clean <- threads_words %>%
  filter(!word %in% stop_words$word) %>%
  filter(!is.na(word))
threads_words_count <- threads_words_clean %>% count(word, sort = TRUE)
```

Processes the data by uniting columns, tokenizing text, filtering, and counting word occurrences.

# TF-IDF Analysis

```r
threads_words_tf_idf <- threads_words_clean %>%
  count(url, word, sort = TRUE) %>%
  bind_tf_idf(word, url, n) %>%
  group_by(word) %>%
  summarise(tf_idf_sum = sum(tf_idf)) %>%
  arrange(desc(tf_idf_sum))
```

Calculates the TF-IDF values for the words in the dataset.

# Bigram Analysis

```r
threads_bigram <- threads_tbl %>%
  unnest_tokens(bigram, text, token = "ngrams", n = 2) %>%
```

Performs bigram analysis by tokenizing the text into bigrams.

# Dependencies

- tidyverse
- tidytext
- wordcloud
- RColorBrewer

# License

This project is licensed under the MIT License.
