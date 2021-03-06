---
title: Fisher's Exact Test
hidden: true
---
## [Return to R tutorials list](%base_url%/?r-language)

# Fisher's Exact Test

```{r, echo=FALSE}
# If instructor copy, use INST = TRUE to see inline code output.
library(knitr)
INST = TRUE

if (INST == TRUE) opts_chunk$set(fig.keep = 'all', results = 'markup', echo = TRUE)
if (INST == FALSE) opts_chunk$set(fig.keep = 'none', results = 'hide', echo = FALSE)

```

![](%theme_url%/img/Fishers_exact_test_Q1_image.jpg)

Some people notice a distinctive smell from their urine after eating asparagus, while others never notice the smell. These differences could arise from variation among people in the chemical profile of the urine (i.e., how compounds from asparagus are metabolised), or from variation in the ability of different people to detect the smell.

A paper (Pelchat *et al.* 2010 *Chemical Senses*) reviewed these studies and presented the following data that described variation among four study populations:

Israel (Lison et al. 1980), 328, 0
China (Hoffenberg 1983), 96, 2
USA (Sugarman and Neelon 1985), 10, 5
USA (Lison et al. 1980), 11, 10

```{r}
asparagus = matrix(c(328,96,10,11,0,2,5,10), nrow=4, dimnames = list(c("Israel", "China", "USA.1", "USA.2"), c("Can detect odour","Cannot detect odour")))
asparagus

```

**Q1**  What numbers of people would be expected to smell the odour in each population if all study populations had the same proportion of people able to detect the odour?

```{r}
chisq.test(asparagus, correct=F)$expected

```

**Q2**  What statistical test could you use to detect differences among populations in the perception of the odour? Ensure you check that the assumptions of the test are met.

**Q3** Run your chosen test. What is your P value?

```{r}
fisher.test(asparagus)

```

# [Go to next page - Fisher Exact Test - 2](%base_url%/?fisher-exact-test2)


`Last updated: May 15,  2020`