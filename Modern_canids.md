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

Data frame of modern canids related to breeds, IGF1 serum level (nmol/L), Body mass (kg) and IGF1-AS genotype:

```{r}
Modern_canids_data <- data.frame(
  Sample_ID = c(37892, 37894, 37896, 37898, 37900, 37902, 37906, 37908, 37910, 37912, 37917, 37919, 37921, 37923, 37929, 37931, 37933, 37935, 37937, 37939, 37941, 37943, 37945, 37947, 37949, 37953, 37955, 37957, 37959, 37961, 37963, 37965, 37968, 37969, 37990, 37992, 37994, 37998, 38000, 38002, 38004, 38006, 38008, 38010, 38012, 38014, 38016, 38018, 38020, 38022, 38024),
  Breed = c("Dachshund", "Chihuahua", "Mixed Breed Dog", "Beagle", "Mixed Breed Dog", "Shetland Sheepdog", "West Highland White Terrier", "Pembroke Welsh Corgi", "Mixed Breed Dog", "Toy Poodle", "Shih tzu", "Labrador Retriever", "Labrador Retriever", "Puggle", "Mini Australian Shephard", "German shepherd x", "Labrador Retriever", "Chihuahua", "Stabyhoun", "1/4 Saluki x 3/4 Alaskan sled dog", "Alaskan sled dog", "Labrador x Golden cross", "Beagle x", "1/2 Saluki x 1/2 Alaskan sled dog", "1/4 Saluki x 3/4 Alaskan sled dog", "1/4 Saluki x 3/4 Alaskan sled dog", "1/2 Beagle x 1/4 lab x 1/4 Plott hound", "1/2 Beagle x 1/4 lab x 1/4 Plott hound", "Bernese Mountain Dog", "Bernese Mountain Dog", "Saluki", "1/4 Saluki x 3/4 Alaskan sled dog", "Shetland Sheepdog", "Shetland Sheepdog", "Scottish Deerhound", "Scottish Deerhound", "Scottish Deerhound", "Soft Coated Wheaten Terrier", "Soft Coated Wheaten Terrier", "Norwich Terrier", "Norwich Terrier", "Norwich Terrier", "Norwich Terrier", "Norwich Terrier", "Bernese Mountain Dog", "Scottish Deerhound", "Scottish Deerhound", "Scottish Deerhound", "Bernese Mountain Dog", "Bernese Mountain Dog", "Bernese Mountain Dog"),
  IGF1_serum_lvl_nmol_L = c(18, 28, 37, 47, 48, 57, 63, 49, 45, 54, 18, 62, 44, 51, 38, 57, 57, 17, 46, 31, 35, 59, 36, 31, 33, 43, 24, 23, 146, 56, 34, 52, 78, 74, 76, 37, 53, 78, 61, 40, 31, 40, 51, 58, 128, 103, 88, 62, 174, 72, 82),
  Body_mass_kg = c(3.4, 4.7, 3.7, 13.5, 15, 12, 8.7, 11.6, 5.4, 8.1, 7.9, 31.8, 31.8, 11.3, 12.2, 31.8,33.6, 3.2, 18.1, 22.68, 20.41, 34, 20.4, 20.4, 18.1, 18.1, 11.3, 9.1, 36.92, 46.86, 27.2, 16.78, 16.78, 15.87, 39.92, 36.29, 40.82, 20.41, 19.95, 4.08, 4.08, 4.54, 6.08, 5.99, 51.71, 48, 41, 43, 45.35, 40.8, 38.5),
  IGF1_AS_Genotype= c("CC", "CC", "CT", "CC", "CC", "CC", "CC", "TT", "CC", "CC", "CC", "TT", "CT", "CC", "CT", "CT", "CT", "CC", "CC", "TT", "TT", "TT", "CC", "CT", "CT", "TT", "CT", "CT", "TT", "TT", "CT", "CT", "TT", "CT", "TT", "TT", "TT", "CC", "CC", "CC", "CC", "CT", "CC", "CC", "TT", "TT", "TT", "TT", "TT", "TT", "TT")
)

Modern_canids_data

```

## Evaluation of the distribution of body mass in relation to the IGF1-AS genotype variant

### Descriptive statistics

To evaluate the distribution of body mass in relation to the IGF1-AS genotype variant, we need to calculate de mean, median and quartiles to construct a box plot as it follows:
```{r}
DESCRIPTIVE_1 <- Modern_canids_data %>%
  group_by(IGF1_AS_Genotype) %>%
  summarise(
    mean = mean(Body_mass_kg, na.rm = TRUE),
    median = median(Body_mass_kg, na.rm = TRUE),
    std = sd(Body_mass_kg, na.rm = TRUE),
    min = min(Body_mass_kg, na.rm = TRUE),
    max = max(Body_mass_kg, na.rm = TRUE)
  )
DESCRIPTIVE_1
```

### Buildind the plot

```{r}
Figure_1 <- ggplot(Modern_canids_data, aes(x = IGF1_AS_Genotype, y = Body_mass_kg, fill=IGF1_AS_Genotype)) +
  geom_violin(trim = TRUE, 
              alpha = 0.5) +
  geom_boxplot(width = 0.2)+theme_light()+
  labs(x = "IGF1-AS genotypes", y = "Body Mass (kg)", title = "Distribution of body mass in relation to the IGF1-AS genotype variant") +
  theme(plot.title = element_text(hjust = 0.5))+
  scale_fill_manual(values=met.brewer("Paquin",3))+ guides(fill = guide_legend(title = "IGF1-AS genotype variant"))
Figure_1
```


### Saving the figures

```{r}
tiff("Figure 1.tiff", width = 604)
  Figure_1
dev.off()
```

```{r}
png("Figure 1.png", width = 604)
  Figure_1
dev.off()
```

```{r}
pdf("Figure 1.pdf")
 Figure_1
dev.off()
```

```{r}
svg("Figure 1.svg")
 Figure_1
dev.off()
```

## Correlating body mass with serum IGF-1 levels

### Descriptive statistics

To assess the correlation between body mass data (Kg) and serum IGF-1 expression levels (nmol/L), it is necessary to calculate Pearson's correlation

```{r}
# Pearson's correlation
cor.test(Modern_canids_data$Body_mass_kg, Modern_canids_data$IGF1_serum_lvl_nmol_L, method = "pearson")
```

### Buildind the plot

```{r}
Figure_2 <- ggplot(data = Modern_canids_data, 
                   aes(x = Body_mass_kg, 
                       y = IGF1_serum_lvl_nmol_L)) +
  labs(x = "Body mass (kg)", y = "IGF1 serum level (nmol/L)") +
  geom_point(aes(fill = IGF1_AS_Genotype),
             size = 3, shape = 21, alpha = 0.7, color = "black") +
  geom_text(x = 40, y = 21, 
            label = "r = 0.635, t(calc.) = 5.763", 
            color = "black", size = 5) +
  geom_smooth(method = lm, se = FALSE, color = "black", linetype = "dashed") +
  theme_light() + 
  labs(title = "Correlation between body mass data (Kg) 
and serum IGF-1 expression levels (nmol/L)") +
  theme(plot.title = element_text(hjust = 0.5)) +
  scale_fill_manual(values = met.brewer("Paquin", 3)) + 
  guides(fill = guide_legend(title = "IGF1-AS genotype variant"))

Figure_2
```

### Saving the figures

```{r}
tiff("Figure 2.tiff", width = 604)
  Figure_2
dev.off()
```

```{r}
png("Figure 2.png", width = 604)
  Figure_2
dev.off()
```

```{r}
pdf("Figure 2.pdf")
 Figure_2
dev.off()
```

```{r}
svg("Figure 2.svg")
 Figure_2
dev.off()
```

