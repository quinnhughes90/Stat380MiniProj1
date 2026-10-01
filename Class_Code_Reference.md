# Class Examples and Project Code

## Step 1: Selecting and renaming

**Example from [STAT380_L7_Linear_RegressionWITHCODE.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L7_Linear_RegressionWITHCODE.qmd:73):**

```r
gdp_2025<- gdp_over_time|>
  select(geo, name, `2025`)
#note the quotes on 2025 to get R to treat as character not number
life_exp_2025<-life_exp_over_time |>
  select(geo, name, `2025`)
country_subset<- countries|>
  select(country, world_4region, landlocked)
head(gdp_2025)
head(life_exp_2025)
head(country_subset)
```

**Example from [STAT380_L7_Linear_RegressionWITHCODE.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L7_Linear_RegressionWITHCODE.qmd:89):**

```r
gdp_2025<-gdp_2025|>
  rename(gdp=`2025`)
#note the quotes again
life_exp_2025<-life_exp_2025|>
  rename(life_exp=`2025`)
#note the quotes again
head(gdp_2025)
head(life_exp_2025)
#could rename country to geo before joining, but it isn't necessary
```

**Our project code:**

```r
cell_2010 <- cell_per_100 |>
  select(geo, `2010`) |>
  rename(cell_phones_per_100_ppl = `2010`)

engy_2010 <- engy_use_pctg |>
  select(geo, `2010`) |>
  rename(residential_energy_use_pctg = `2010`)

hdi_2010 <- hdi |>
  select(geo, `2010`) |>
  rename(human_development_ind = `2010`)

mat_mort_2010 <- maternal_mortality |>
  select(geo, `2010`) |>
  rename(maternal_mortality = `2010`)

daily_inc_2010 <- daily_inc |>
  select(geo, `2010`) |>
  rename(daily_income = `2010`)

landlocked <- gap |>
  select(geo, name, landlocked)
```

## Step 1: Combining datasets

**Example from [STAT380_L5_Joins.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L5_Joins.qmd:84):**

```r
Superheros |>
  full_join( Publishers, by="publisher")
```

**Example from [STAT380_L7_Linear_RegressionWITHCODE.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L7_Linear_RegressionWITHCODE.qmd:105):**

```r
gapminder_2025<- gdp_2025|>
  inner_join(life_exp_2025, by=c("geo", "name"))|> #note you need both names as otherwise you end up with two name or geo coloumns.
  inner_join(country_subset, by=c("geo"="country"))
  
head(gapminder_2025)
```

**Our project code:**

```r
full_2010 <- cell_2010 |>
  full_join(engy_2010, by = "geo") |>
  full_join(landlocked, by = "geo") |>
  full_join(hdi_2010, by = "geo") |>
  full_join(mat_mort_2010, by = "geo") |>
  full_join(daily_inc_2010, by = "geo") |>
  select(geo, name, cell_phones_per_100_ppl,
         residential_energy_use_pctg, landlocked,
         human_development_ind, maternal_mortality, daily_income)
```

## Step 1: Removing incomplete rows

No demonstrated example in the supplied lectures. The existing `na.omit()` code is retained.

**Our project code:**

```r
clean_2010 <- full_2010 |>
  na.omit()
```

## Step 1: Saving the cleaned data

**Example from [STAT380_L3_Data_Import.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L3_Data_Import.qmd:96):**

```r
write_csv(WVB_csv, "WVB_PSU_2025_Player_Stats_Off_csv.csv")
```

**Our project code:**

```r
write_csv(clean_2010, "clean_2010.csv")
```

## Step 3: Summary statistics

**Example from [STAT380_L4_Wrangling.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L4_Wrangling.qmd:120):**

```r
austin_12 |> 
  summarize(avg_Sales = mean(sales), 
                        sd_sales = sd(sales), 
                        min_vol = min(volume), 
                        max_vol = max(volume), 
                        mdn_list = median(listings), 
                        iqr_list = IQR(listings),
                        sample_size = n())
```

**Our project code:**

```r
clean_2010 |>
  summarise(mean   = mean(daily_income),
            median = median(daily_income),
            sd     = sd(daily_income),
            min    = min(daily_income),
            max    = max(daily_income))
```

**Our project code:**

```r
clean_2010 |>
  summarise(mean   = mean(cell_phones_per_100_ppl),
            median = median(cell_phones_per_100_ppl),
            sd     = sd(cell_phones_per_100_ppl),
            min    = min(cell_phones_per_100_ppl),
            max    = max(cell_phones_per_100_ppl))
```

**Our project code:**

```r
clean_2010 |>
  summarise(mean   = mean(residential_energy_use_pctg),
            median = median(residential_energy_use_pctg),
            sd     = sd(residential_energy_use_pctg),
            min    = min(residential_energy_use_pctg),
            max    = max(residential_energy_use_pctg))
```

**Our project code:**

```r
clean_2010 |>
  summarise(mean   = mean(maternal_mortality),
            median = median(maternal_mortality),
            sd     = sd(maternal_mortality),
            min    = min(maternal_mortality),
            max    = max(maternal_mortality))
```

**Our project code:**

```r
clean_2010 |>
  summarise(mean   = mean(human_development_ind),
            median = median(human_development_ind),
            sd     = sd(human_development_ind),
            min    = min(human_development_ind),
            max    = max(human_development_ind))
```

## Step 3: Countries with low and high values

**Example from [STAT380_L4_Wrangling.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L4_Wrangling.qmd:135):**

```r
#| eval = F
austin_12 |>
  arrange(volume)
```

**Example from [STAT380_L4_Wrangling.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L4_Wrangling.qmd:59):**

```r
#| eval = F
txhousing |> 
  select(sales, volume)
```

**Example from [STAT380_L3_Data_Import.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L3_Data_Import.qmd:39):**

```r
head(WVB_PSU_2025_Player_Stats_Off_csv)
head(WVB_PSU_2025_Player_Stats_Off_Xcel)
```

**Example from [STAT380_L4_Wrangling.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L4_Wrangling.qmd:43):**

```r
tail(txhousing)
```

**Our project code:**

```r
# Six countries with the lowest values
clean_2010 |>
  arrange(daily_income) |>
  select(name, geo, daily_income) |>
  head()

# Six countries with the highest values (displayed in ascending order)
clean_2010 |>
  arrange(daily_income) |>
  select(name, geo, daily_income) |>
  tail()
```

**Our project code:**

```r
# Six countries with the lowest values
clean_2010 |>
  arrange(cell_phones_per_100_ppl) |>
  select(name, geo, cell_phones_per_100_ppl) |>
  head()

# Six countries with the highest values (displayed in ascending order)
clean_2010 |>
  arrange(cell_phones_per_100_ppl) |>
  select(name, geo, cell_phones_per_100_ppl) |>
  tail()
```

**Our project code:**

```r
# Six countries with the lowest values
clean_2010 |>
  arrange(residential_energy_use_pctg) |>
  select(name, geo, residential_energy_use_pctg) |>
  head()

# Six countries with the highest values (displayed in ascending order)
clean_2010 |>
  arrange(residential_energy_use_pctg) |>
  select(name, geo, residential_energy_use_pctg) |>
  tail()
```

**Our project code:**

```r
# Six countries with the lowest values
clean_2010 |>
  arrange(maternal_mortality) |>
  select(name, geo, maternal_mortality) |>
  head()

# Six countries with the highest values (displayed in ascending order)
clean_2010 |>
  arrange(maternal_mortality) |>
  select(name, geo, maternal_mortality) |>
  tail()
```

**Our project code:**

```r
# Six countries with the lowest values
clean_2010 |>
  arrange(human_development_ind) |>
  select(name, geo, human_development_ind) |>
  head()

# Six countries with the highest values (displayed in ascending order)
clean_2010 |>
  arrange(human_development_ind) |>
  select(name, geo, human_development_ind) |>
  tail()
```

## Step 3: Existing distribution plots and category counts

`table()` and `geom_bar()` have no demonstrated example in the supplied lectures and are retained. L7 mentions `geom_histogram()` only in the commented example below; `bins = 30` is retained from the original project.

**Example from [STAT380_L7_Linear_RegressionWITHCODE.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L7_Linear_RegressionWITHCODE.qmd:342):**

```r
gapminder_2025|>
  group_by(landlocked)|>
  summarize("average life exp"=mean(life_exp, na.rm=TRUE))

#"gapminder_2025|>
  #group_by(mapping=aes(x=life_exp))|>
  #geom_histogram() +facet_wrap(~landlocked)"
```

**Our project code:**

```r
table(clean_2010$landlocked)

ggplot(clean_2010, aes(x = landlocked)) +
  geom_bar() +
  labs(title = "Landlocked vs. Coastline Countries (2010)",
       x = "Landlocked", y = "Number of Countries")
```

**Our project code:**

```r
ggplot(clean_2010, aes(x = daily_income)) +
  geom_histogram(bins = 30) +
  labs(title = "Daily Income (2010)", x = "Daily Income", y = "Count")
```

**Our project code:**

```r
ggplot(clean_2010, aes(x = cell_phones_per_100_ppl)) +
  geom_histogram(bins = 30) +
  labs(title = "Cell Phones per 100 People (2010)", x = "Cell Phones", y = "Count")
```

**Our project code:**

```r
ggplot(clean_2010, aes(x = residential_energy_use_pctg)) +
  geom_histogram(bins = 30) +
  labs(title = "Residential Energy Use Percentage (2010)", x = "Residential Energy Use", y = "Count")
```

**Our project code:**

```r
ggplot(clean_2010, aes(x = maternal_mortality)) +
  geom_histogram(bins = 30) +
  labs(title = "Maternal Mortality (2010)", x = "Maternal Deaths per 100,000 Births", y = "Count")
```

**Our project code:**

```r
ggplot(clean_2010, aes(x = human_development_ind)) +
  geom_histogram(bins = 30) +
  labs(title = "Human Development Index (2010)", x = "HDI", y = "Count")
```

## Step 4: Income summaries by landlocked status

**Example from [STAT380_L7_Linear_RegressionWITHCODE.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L7_Linear_RegressionWITHCODE.qmd:342):**

```r
gapminder_2025|>
  group_by(landlocked)|>
  summarize("average life exp"=mean(life_exp, na.rm=TRUE))

#"gapminder_2025|>
  #group_by(mapping=aes(x=life_exp))|>
  #geom_histogram() +facet_wrap(~landlocked)"
```

**Example from [STAT380_L4_Wrangling.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L4_Wrangling.qmd:120):**

```r
austin_12 |> 
  summarize(avg_Sales = mean(sales), 
                        sd_sales = sd(sales), 
                        min_vol = min(volume), 
                        max_vol = max(volume), 
                        mdn_list = median(listings), 
                        iqr_list = IQR(listings),
                        sample_size = n())
```

**Our project code:**

```r
clean_2010 |>
  group_by(landlocked) |>
  summarize(countries = n(),
            mean_income = mean(daily_income),
            median_income = median(daily_income),
            sd_income = sd(daily_income))
```

## Step 4: Income distribution by landlocked status

**Example from [STAT380_L7_Linear_RegressionWITHCODE.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L7_Linear_RegressionWITHCODE.qmd:332):**

```r
gapminder_2025|> 
  ggplot(mapping=aes(y=life_exp, x=landlocked))+
  geom_violin()
```

**Our project code:**

```r
clean_2010 |>
  ggplot(mapping = aes(x = landlocked, y = daily_income)) +
  geom_violin() +
  labs(title = "Daily Income by Landlocked Status (2010)",
       x = "Landlocked status", y = "Daily income per person")
```

## Step 4: Existing scatterplot matrix

No demonstrated `pairs()` example in the supplied lectures. The original matrix is retained; the new text interprets its income relationships.

**Our project code:**

```r
pairs(clean_2010[c(
  "cell_phones_per_100_ppl",
  "residential_energy_use_pctg",
  "human_development_ind",
  "maternal_mortality",
  "daily_income"
)])
```

## Step 5: Creating transformed variables with mutate

**Example from [STAT380_L7_Linear_RegressionWITHCODE.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L7_Linear_RegressionWITHCODE.qmd:143):**

```r
gapminder_2025<-gapminder_2025|>
  mutate(log_gdp=log10(gdp)) 
#note default base is e (ln, natural log) hence the log10, which is easier to interpret. Both work fine statistically.
```

L7 demonstrates `mutate()` with `log10()` and discusses natural logs in its comment. Our existing natural-log transformations are retained. `log1p()`, `factor()`, and `pairs()` are not demonstrated in the supplied lecture files; they remain unchanged. L7 demonstrates `summary()` on a model, not the data-frame summary retained here.

**Our project code:**

```r
transformed_2010 <- clean_2010 |>
  mutate(
  log_daily_income = log(daily_income),
  log_energy = log1p(residential_energy_use_pctg),
  log_mortality = log1p(maternal_mortality),
  landlocked = factor(landlocked)
)

pairs(transformed_2010[c(
  "cell_phones_per_100_ppl",
  "log_energy",
  "human_development_ind",
  "log_mortality",
  "log_daily_income"
)])

summary(transformed_2010)
```

## Step 6: Multiple regression models

**Example from [STAT380_L8_Multi_Linear_Regression.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L8_Multi_Linear_Regression.qmd:68):**

```r
firstmultiv=gapminder_2025|>
  lm(life_exp~gdp+world_4region, data= _) #base R code, the _ tells R where to pipe
firstmultiv
```

**Example from [STAT380_L8_Multi_Linear_Regression.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L8_Multi_Linear_Regression.qmd:98):**

```r
summary(firstmultiv)
```

**Example from [STAT380_L8_Multi_Linear_Regression.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L8_Multi_Linear_Regression.qmd:90):**

```r
plot(firstmultiv, which = 1)
plot(firstmultiv, which = 2)
```

The existing `par(mfrow = ...)` layout and full default diagnostic set are retained; the lectures demonstrate separate residual and Q–Q plots. This step adds writing only.

**Our project code:**

```r
income_model1 <- lm(
  log_daily_income ~ cell_phones_per_100_ppl +
    log_energy +
    human_development_ind +
    log_mortality +
    landlocked,
  data = transformed_2010
)

summary(income_model1)

par(mfrow = c(2, 2))
plot(income_model1)
par(mfrow = c(1, 1))
```

**Our project code:**

```r
income_model2 <- lm(
  log_daily_income ~ log_energy +
    landlocked,
  data = transformed_2010
)

summary(income_model2)

par(mfrow = c(2, 2))
plot(income_model2)
par(mfrow = c(1, 1))
```

## Step 6: Simple regression model

**Example from [Stat380_L6_SLRBasics.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/Stat380_L6_SLRBasics.qmd:106):**

```r
linearModel=lm(Calories~Carbs, data= Cereal) #base, we'll do tidymodels in next example
```

**Example from [STAT380_L7_Linear_RegressionWITHCODE.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L7_Linear_RegressionWITHCODE.qmd:194):**

```r
summary(lifegdp)
```

**Example from [STAT380_L7_Linear_RegressionWITHCODE.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L7_Linear_RegressionWITHCODE.qmd:186):**

```r
plot(lifegdp, which = 1)
plot(lifegdp, which = 2)
```

**Our project code:**

```r
income_model3 <- lm(
  log_daily_income ~ human_development_ind,
  data = transformed_2010
)

summary(income_model3)

par(mfrow = c(2, 2))
plot(income_model3)
par(mfrow = c(1, 1))
```

## Step 7: Comparing the models

Source: [STAT380_L8_Multi_Linear_Regression.qmd — quality of fit and model-selection discussion](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L8_Multi_Linear_Regression.qmd:124).

This step adds a written comparison using adjusted R-squared, residual standard error, diagnostic plots, and simplicity. The values come from the existing `summary(income_model1)`, `summary(income_model2)`, and `summary(income_model3)` calls documented in Step 6. No new R code was added.

## Step 8: Our question and grouped scatterplot

**Example from [STAT380_L8_Multi_Linear_Regression.qmd](/Users/quinnhughes/Library/CloudStorage/OneDrive-ThePennsylvaniaStateUniversity/yr3/stat380/STAT380_L8_Multi_Linear_Regression.qmd:40):**

```r
gapminder_2025 |>
  ggplot(mapping=aes(x=log_gdp, y=life_exp, color=world_4region))+
  geom_point()+
  labs(title="Life Expectancy vs.  GDP per capita by country for 2025", x="GDP per capita", y="Years", color="World Region")
```

**Our project code:**

```r
transformed_2010 |>
  ggplot(mapping = aes(x = human_development_ind,
                       y = log_daily_income,
                       color = landlocked)) +
  geom_point() +
  labs(title = "HDI and Daily Income by Landlocked Status (2010)",
       x = "Human Development Index (HDI)",
       y = "Log daily income (natural log)",
       color = "Country type")
```
