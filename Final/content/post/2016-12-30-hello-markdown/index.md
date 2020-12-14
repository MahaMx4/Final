---
date: "2016-12-30T21:49:57-07:00"
title: Methodology 
---

# Methodology

## Data

  The data set was simulated using an observational study conducted by Demirici et al. in Turkey that tracked the smartphone usage of 319 university students and used the Pittsburgh Sleep Quality Index to assess sleep quality (Demirici et al.,2015). I simulated the proportion of male and female for $gender$ based on the proportion that was found in the study where it was 63.6% female and 36.4% male.Furthermore, $age$ was simulated based on all the participants being university students. Additionally, $smartphoneusage$ was simulated through this study since the researchers used a smartphone addiction scale where the students were classified based on not using a phone, low usage and high usage. I simulated $familyincome$ which was not provided in the observational study and is written in the thousands, but rather it was comprised of data collected by Caner & Okten on higher education in Turkey (Caner & Okten, 2013).This study included data on public university students in Turkey which aligns Demeric et al.'s study. Family income provides economic context on smartphone usage. In Demirici at al.'s study, only students who agreed to participate were included and this study also included people who did not use a smartphone as the control. The target population is university students, the frame population is Turkish university students enrolled in the public university, Süleyman Demirel University, and the sampled population was the 319 students who participated in the observational study.
  
## Model

The first model used is a logistic regression model which provides an explanation as to if a person was treated as a function of sleep quality like we expect them to be. A logistic regression model is being used as sleep quality is a binary outcome where 0 means that the individual sleeps well whereas 1 means that they have trouble sleeping.

The model is as follows:
$$log(\frac p{1-p})= \beta_{0} + \beta_{1}x_{age} + \beta_{2}x_{smartphoneusage} +\beta_{3}x_{gender} +\beta_{4}x_{familyincome}$$
 where, 
 $$ log (\frac p{1-p}) $$ is the probability that sleep quality is affected by phone usage, $\beta_{0}$ is the population y intercept,$\beta_{1}$, $\beta_{2}$ , $\beta_{3}$, $\beta_{4}$ are the population state coefficient for each of the variables,and $e_i$ is the random  error term (Changbao et.al,2019).
 
```{r,include=FALSE}
propensity_score <- glm(sleep_quality ~ age + smartphoneusage + gender + familyincome, 
                        family = binomial,
                        data = phone_addiction_data)
summary(propensity_score)


  

```

  The second and last model used was to examine the effect that being treated (having sleep issues) has on average sleep that occurs normally. A simple linear regression model is being used as there is a linear relationship between these variables and the dependent variable, average sleep.
  
   The model is:
   $$Y_{i}  = \beta_{0} + \beta_{1}x_{age} + \beta_{2}x_{genderMale} +\beta_{3}x_{sleepquality} +\beta_{4}x_{smartphoneusageLow}+ e_i $$
     Where $y_i$ is the value of the study for the ith observation, $y_i$ is the vector of inputs for the ith observation  which in this case is average sleep ,$\beta_{0}$ is the population y intercept,$\beta_{1}$, $\beta_{2}$ , $\beta_{3}$, $\beta_{4}$ are the population state coefficient for each of the variables,and $e_i$ is the random  error term (Changbao et.al,2019).
     
## Results

#Phone Addiction Data

Table 1: The first few data entries of the phone addiction data that was simulated

```{r, echo=FALSE}
mydata <- head (phone_addiction_data1)

knitr::kable(mydata, align = "lccrr")

```

This demonstrates the data that was simulated where sleep quality is 0 representing not having the treatment, bad sleep and 1 is the teratment. Furthermore, family income is in the thousands.
## Model Checks

For the  logistic regression model these are the following model checks ensuring that this is the correct model:
 
```{r, echo=FALSE}

phone_addiction_data %>% 
  ggplot(aes(x=age,y=sleep_quality))+
  geom_point()

phone_addiction_data %>% 
  ggplot(aes(x=smartphoneusage,y=sleep_quality))+
  geom_point()


```
As all of these graphs demonstrate a straight line parallel to another straight line, this indicates that this model was justified. 


## Propensity Score

Table 2: Summary of the Findings of the Logistic Regression Moedl

```{r propensity score,echo=FALSE}



propensity_score <- glm(sleep_quality ~ age + smartphoneusage + gender + familyincome, 
                        family = binomial,
                        data = phone_addiction_data)

data <- tidy(propensity_score)
knitr::kable(data, align = "lccrr")
```

This demonstrates that the findings were statistically significant as all of th p values.  are less than 0.05. Additionally, it demonstrates a positive relationship between sleep quality and age, low or no smartphone usage, and male individuals. Whereas there is a negative relationship between sleep quality and  family income. 

Table 3: Treated vs Untreated Values

```{r,echo=FALSE}
treated<-table(phone_addiction_data$sleep_quality)

knitr::kable(treated, align = "lccrr")

```
This table demonstrates that 8 students were in the treatment group whereas 311 were not in this group.

Table 4: Summary of the Findings of the Simple Linear Regression Model
```{r,echo=FALSE}

propensity_score_regression<-
  lm(average_sleep~age+ smartphoneusage + gender+ familyincome +sleep_quality,
     data=phone_addiction_data_matched)


huxtable::huxreg(propensity_score_regression)


```

This demonstrates that a non statistifically significant increase in age as every year, the average sleep decreases by 1.069. Additionally, as smartphone usage for those in the low category increases by one unit, the average sleep decreases by 0.226. Moreover, as gender transitions from male to female the average sleep decreases by 2.590 indicating that females have worse sleep quality than males. Additionally, family income did not have any impact on sleep quality.

# Discussion

## Summary

  To begin with, phone addiction has greater implications on sleep quality which directly affects human health. The data was simulated from Demrici et al.'s 2015 study on sleep quality and phone usage in 319  Turkish university students. A logistic regression model was used to demonstrate that there was a correlation between sleep quality and phone usage as the results indicated that it was statistically significant. Additionally, model checks demonstrated that this was the accurate model for this analysis. Furthermore, propensity score was used to convert this observational study into a randomized controlled trial to indicate causation. This demonstrated that 8 students had the treatment, bad sleep quality, whereas 311 students were not treated. A linear regression model showed that there was not any statistically significant findings. However, the non significant findings were that as age increases by one year, the average sleep decreases by 1.069. Additionally, it demonstrated that females had worse sleep quality than males and that family income did not have an impact on average sleep. It was further shown that low smartphone usage is correlated with an increase in average sleep. These results indicate that phone usage does not have a statistically significant impact on sleep quality in university aged individuals. 

## Conclusions

  Furthermore, as these results were not significant comparing it to that of the studies used to simulate the data can lead to a better understanding. The data simulated included family income from Caner & Okten on university students in Turkey (Caner & Okten, 2013) . This was added because it provided information regarding if economic background played a relevant role in phone usage. It was shown that there was no impact of family income on average sleep.  This can be attributed to all of the participants being in public university, which tends to mean in Turkey that they are from a higher income family (Caner& Okten, 2013). Therefore, although they may have a lower family income in comparison to someone else, in the greater scheme of the population, they are still on the higher end of the income scale. The results align with Demirici et al's study as their study was statistically significant and showed that as a student became older the average sleep decreased (Demirici et al., 2015). Additionally, this study showed that females were more likely to have less sleep than males (Demirici et al., 2015). Furthermore, it demonstrated that high smartphone usage had a significant link with decreased sleep quality (Demirici et al.,2015) . Overall, this demonstrates that this simulation was effective and further proves the link between phone addiction and a negative impact on sleep

  These findings have greater implications on the world as smart phone usage has significantly increased through the years. In Korea, mobile phone technology has advanced beyond any other country in the world and this has led to this country having the most people using mobile phone with 66% of the population (Park, 2005). Within Korea, 73% of participants in a study on smartphone usage, said that if they do not use their phone for a few hours they feel isolated and uncomfortable (Park, 2005). This phone dependency highlights how using your phone has become second nature to many people. Thus, demonstrating how allowing people to have options of activities that do not require a phone, but are just as entertaining can allow this dependency to be mitigated. Furthermore, phone usage has been linked in other studies to making people feel more tired and has found to be associated with health outcomes associated with sleep deprivation (Van de Bulck, 2017). Specifically, it has been associated with shorter sleep periods, poorer sleep quality, insomnia, and sleepiness during the day (Munezawa, 2011). It also has been further shown that young adults are more likely to use their smartphone, however they also are in school or work which can also make them more tired (Exelmans & Van de Bulck, 2016). This emphasizes that the tiredness associated with young adults can not be solely due to smartphone usage. This highlights the importance of programs focused towards promoting young adults to not use their phone past bedtime as that is associated with a higher tendency of poor sleep quality, but rather encourage other activities that include unwinding before bed. Ultimately, this will improve quality of live and decrease the negative health effects brought upon by a lack of good quality sleep. Therefore, this highlights how more research should be conducted on the impact of phone usage and sleep quality for there to be conclusive evidence.

  
## Weakness & Next Steps

As the data in this study was simulated based on an observational study conducted by Demerici et al., the limitations of this analysis is analogous to the limitations of the study. The limitations of the observational study is that there was a small population as it only included 319 participants. Therefore, doubling the sampled population or tripling it can lead to more representative results. Furthermore, the target population was university students which means that these results can only be applied to this age group. While, this age group is justified by studies showing that individuals in this age group are more likely to be using phones than other age groups. However, excluding young adults not in university means that this study is only made up of educated adults and is not representative of everyone in the age group. Rather, more studies expanding this target population to people not in university can lead to more conclusive and representative results.

Next steps of this analysis can be to conduct this same study and analysis in Korea, which is a country with different cultural beliefs than Turkey. Therefore, this can allow for a comparison between these two other countries. This can lead to a discussion towards the cultural lifestyle in both countries and can lead to a conclusion towards whether or not phone usage is dependent on the country. This can extend to countries near Turkey or anywhere around the world to see if phone usage impacting sleep quality is a global problem and not localized to a few countries. Further more recent studies on sleep quality and phone usage can provide more information on this topic.


# Bibliography
Alexander, R. (2020). Running Through a Propensity Score Matching Example. Code.

Austin, P. C. (2011). An introduction to propensity score methods for reducing the effects   of confounding in observational studies. Multivariate behavioral research, 46(3), 399-424.

Caner, A., & Okten, C. (2013). Higher education in Turkey: Subsidizing the rich or the       poor?. Economics of Education Review, 35, 75-92.

Changbao W, Thompson M. 2019.Sampling Theory and Practice. Springer. 10 p.

Demirci, K., Akgönül, M., & Akpinar, A. (2015). Relationship of smartphone use severity      with  sleep quality, depression, and anxiety in university students. Journal of            behavioral   addictions, 4(2), 85-92.  

Exelmans, L., & Van den Bulck, J. (2016). Bedtime mobile phone use and sleep in adults. Social Science & Medicine, 148, 93-101.

Grandner, M. A. (2017). Sleep, health, and society. Sleep medicine clinics, 12(1), 1-22.

Hirshkowitz, M., Whiton, K., Albert, S. M., Alessi, C., Bruni, O., DonCarlos, L., ... &      Neubauer, D. N. (2015). National Sleep Foundation’s sleep time duration recommendations:   methodology and results summary. Sleep health, 1(1), 40-43.

Ibrahim, N. K., Baharoon, B. S., Banjar, W. F., Jar, A. A., Ashor, R. M., Aman, A. A., &     Al-Ahmadi, J. R. (2018). Mobile phone addiction and its relationship to sleep quality and   academic achievement of medical students at King Abdulaziz University, Jeddah, Saudi       Arabia. Journal of research in health sciences, 18(3), e00420.


Munezawa, T., Kaneita, Y., Osaki Y., Kanda, H., Minowa, M., Suzuki K., et al. (2011).
 The association between use of mobile phones after lights out and sleep disturbances among  Japanese adolescents: a nationwide cross-sectional survey. Sleep, 34 (8), pp.      1013-1020.

Park, W. K. (2005). Mobile phone addiction. In Mobile communications (pp. 253-272).          Springer, London.

Sahin, S., Ozdemir, K., Unsal, A., & Temiz, N. (2013). Evaluation of mobile phone addiction  level and sleep quality in university students. Pakistan journal of medical sciences,      29(4), 913.

Van den Bulck, J. (2007). Adolescent use of mobile phones for calling and for sending text   messages after lights out: results from a prospective cohort study with a one-year         follow-up. Sleep, 30 (9), pp. 1220-1223.

