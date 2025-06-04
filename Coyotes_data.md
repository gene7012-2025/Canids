# Body size variation in modern canids
According to  [PLASSAIS et al. (2022)](https://doi.org/10.1016/j.cub.2021.12.036), a single non-coding genetic variant in the IGF1 gene plays an important role in regulating body size in both ancient and modern canids. Here, using the data from PLASSAIS et al. (2022) work, we will recreate some of the statistical methods and generate plots that will help in the discussion of the paper's results.

## Install Packages
### R
Install [RStudio](https://posit.co/download/rstudio-desktop/) 

### Packages required for analysis

```{r}
#install.packages("ggplot2")
library(ggplot2)

#install.packages("MetBrewer")
library(MetBrewer)

#install.packages("scales")
library(scales)

#install.packages("tidyverse")
library(tidyverse)

#install.packages("dplyr")
library(dplyr)

Sys.setlocale("LC_CTYPE", "en_US.UTF-8")
```

## Dataframe
Data frame of 28 coyotes from Pennsylvania Body mass (kg) and IGF1-AS genotype:

```{r}
Wild_canides_data <- data.frame(
    Body_mass_kg = c(20.7, 18.8, 23.1, 19.7, 19.5, 13.2, 13.4, 18.7, 12.3, 18.6, 12.8, 11.4, 13.8, 19.5, 13.5, 13.7, 13.4, 13.1, 11.9, 19.9, 18.4, 18.2, 18.9, 19.6, 19.6, 19.7, 20.5, 19.5),
  IGF1_AS_Genotype= c("TT", "TT", "CT", "CT", "TT", "CC", "CT", "CT", "CC", "CC", "CC", "CC", "CT", "CT", "CC", "CT", "CT", "CT", "CC", "TT", "TT", "CC", "TT", "TT", "TT", "TT", "TT", "TT")
)

Wild_canides_data
```

## Evaluation of the distribution of body mass in relation to the IGF1-AS genotype variant

### Descriptive statistics

To evaluate the distribution of body mass in relation to the IGF1-AS genotype variant, we need to calculate de mean, median and quartiles to construct a box plot as it follows:

```{r}
DESCRIPTIVE_2 <- Wild_canides_data %>%
  group_by(IGF1_AS_Genotype) %>%
  summarise(
    mean = mean(Body_mass_kg, na.rm = TRUE),
    median = median(Body_mass_kg, na.rm = TRUE),
    std = sd(Body_mass_kg, na.rm = TRUE),
    min = min(Body_mass_kg, na.rm = TRUE),
    max = max(Body_mass_kg, na.rm = TRUE)
  )
DESCRIPTIVE_2
```

### Buildind the plot
```{r}
Figure_3<- ggplot(Wild_canides_data, aes(x = IGF1_AS_Genotype, y = Body_mass_kg, fill=IGF1_AS_Genotype)) +
  geom_violin(trim = TRUE, 
              alpha = 0.5) +
  geom_boxplot(width = 0.2)+theme_light()+
  labs(x = "IGF1-AS genotypes", y = "Body Mass (kg)", title = "Distribution of body mass in relation to the IGF1-AS genotype variant") +
  theme(plot.title = element_text(hjust = 0.5))+
  scale_fill_manual(values=met.brewer("Paquin",3))+ guides(fill = guide_legend(title = "IGF1-AS genotype variant"))
Figure_3
```

### Saving the figures

```{r}
tiff("Figure 3.tiff", width = 604)
  Figure_3
dev.off()
```

```{r}
png("Figure 3.png", width = 604)
  Figure_3
dev.off()
```

```{r}
pdf("Figure 3.pdf")
 Figure_3
dev.off()
```

```{r}
svg("Figure 3.svg")
 Figure_3
dev.off()
```

