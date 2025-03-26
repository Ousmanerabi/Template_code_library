### Step 1: Install packages
```r
install.packages(c('dplyr', 'readxl', 'tidyverse', 'janitor', 'writexl'))
```
`dplyr`: is a grammar of data manipulation, providing a consistent set of verbs that help you solve the most common data manipulation challenges (ref:https://dplyr.tidyverse.org/).

`readxl`: The readxl package makes it easy to get data out of Excel and into R. Compared to many of the existing packages (e.g. gdata, xlsx, xlsReadWrite) readxl has no external dependencies, 
          so it’s easy to install and use on all operating systems.

`janitor`: has simple functions for examining and cleaning dirty data. The main janitor functions:

perfectly format data.frame column names;
create and format frequency tables of one, two, or three variables - think an improved table(); and
provide other tools for cleaning and examining data.frames.

`tidyverse`:  is designed to make it easy to install and load core packages from the tidyverse in a single command

`writexl`: 

### Step 2: Load the packages already installed
```r
library(dplyr)
library(readxl)
library(tidyverse)
library(janitor)
library(writexl)

```
### Step 3: Import data

#### Step 3.1 : Setup the directory
```r
setwd()
```
` setwd`: file path
#### Step 3.2 : Read a single file: To import a single file, you must first identify its extension (.xls, .xlsx, or .csv).
`For Excel files (.xls and .xlsx), use the `read_excel` function from the `readxl` package.

```r
raw_data = read_excel('file path/filename.xlsx')
```
`For .csv files, the import method depends on the delimiter used:

* Comma-delimited (.csv) file:
```r
raw_data = read.csv('file path/filename.csv')
```
* Semicolon-delimited (.csv) file:

```r
raw_data = read.csv2('file path/filename.csv')
```

#### Step 3.3 : Read multiple files and bind it into a single file
##### option 1: read .xls files
##### Breaking down the code:

`The code below retrieves all .xls files in your working directory and stores them in the object data.
```r

data = list.files(pattern = ".xls", full.names = TRUE)
```
`The code snippet above imports all `.xls` files into the data object and stores them in the `myfiles` object.

```r
myfiles = lapply(data, read_xls)
```
`The code below merges all `.xls` files from the myfiles object into a single data.frame named `raw_data`.
```r
raw_data <- dplyr::bind_rows(myfiles)
```
- #### Note: if ou're using .xlsx, .cvs or .csv2, please change or modify the code 

### Step 4: View the data (description of data)
```r
head()

Str()

glimpse()


```
### Step 5: Select and rename key variables

  This code enables you to select all the variables needed for routine data analysis and rename them accordingly.

  The `clean_names` function replaces spaces in variable names with `underscores (_).
  
  The `separate` function splits the period (date) column into separate `month and `year columns.
  
  The first `mutate` function is used to create new variable month. 
  
  The final `mutate` function is used to convert the month and year into numerical variables.
  

  ##### Make sure to adjust the variable names to match those in your raw_data dataframe.
```r

raw_data = raw_data %>% %>% clean_names()%>%
                dplyr::select(adm1 = orgunitlevel2, adm2, hf = organisationunitname, period =periodname, 
                reprate = consultation_externe_reporting_rate,
                reptime = consultation_externe_reporting_rate_on_time, 
                allout_u5 = c_ext_nombre_de_nouveaux_cas_toutes_causes_confondues_moins_de_5_ans,
                allout_ov5 = c_ext_nombre_de_nouveaux_cas_toutes_causes_confondues_5_ans_et_plus_sans_fe,
                allout_preg = c_ext_nombre_de_nouveaux_cas_toutes_causes_confondues_fe,
                susp_u5 = c_ext_nombre_de_cas_suspects_de_paludisme_moins_de_5_ans,
                susp_ov5 = c_ext_nombre_de_cas_suspects_de_paludisme_5_ans_et_plus_sans_fe,
                susp_preg = c_ext_nombre_de_cas_suspects_de_paludisme_fe,
                test_rdt_u5 = c_ext_nombre_de_tests_de_diagnostic_rapide_tdr_effectues_fs_moins_de_5_ans,
                test_rdt_ov5 = c_ext_nombre_de_tests_de_diagnostic_rapide_tdr_effectues_fs_5_ans_et_plus_sans_fe,
                test_rdt_preg = c_ext_nombre_de_tests_de_diagnostic_rapide_tdr_effectues_fs_fe,
                test_mic_u5 = parasito_nombre_de_ge_fm_effectues_en_interne_moins_de_5_ans,
                test_mic_ov5 = parasito_nombre_de_ge_fm_effectues_en_interne_5_ans_et_plus_sans_fe,
                test_mic_preg = parasito_nombre_de_ge_fm_effectues_en_interne_fe,
                conf_rdt_u5 = c_ext_nombre_de_tests_de_diagnostic_rapide_tdr_positifs_moins_de_5_ans,
                conf_rdt_ov5 = c_ext_nombre_de_tests_de_diagnostic_rapide_tdr_positifs_5_ans_et_plus_sans_fe,
                conf_rdt_preg = c_ext_nombre_de_tests_de_diagnostic_rapide_tdr_positifs_fe,
                conf_mic_pf_u5 = parasito_nombre_de_ge_fm_positifs_a_plasmodium_falciparum_en_interne_moins_de_5_ans,
                conf_mic_pf_ov5 = parasito_nombre_de_ge_fm_positifs_a_plasmodium_falciparum_en_interne_5_ans_et_plus_sans_fe,
                conf_mic_pf_preg = parasito_nombre_de_ge_fm_positifs_a_plasmodium_falciparum_en_interne_fe,
                conf_mic_pm_u5 = parasito_nombre_de_ge_fm_positifs_a_plasmodium_malariae_en_interne_moins_de_5_ans,
                conf_mic_pm_ov5 = parasito_nombre_de_ge_fm_positifs_a_plasmodium_malariae_en_interne_5_ans_et_plus_sans_fe,
                conf_mic_pm_preg = parasito_nombre_de_ge_fm_positifs_a_plasmodium_malariae_en_interne_fe,
                conf_mic_po_u5 = parasito_nombre_de_ge_fm_positifs_a_plasmodium_ovale_en_interne_moins_de_5_ans,
                conf_mic_po_ov5 = parasito_nombre_de_ge_fm_positifs_a_plasmodium_ovale_en_interne_5_ans_et_plus_sans_fe,
                conf_mic_po_preg = parasito_nombre_de_ge_fm_positifs_a_plasmodium_ovale_en_interne_fe,
                conf_mic_pv_u5 = parasito_nombre_de_ge_fm_positifs_a_plasmodium_vivax_en_interne_moins_de_5_ans,
                conf_mic_pv_ov5 = parasito_nombre_de_ge_fm_positifs_a_plasmodium_vivax_en_interne_5_ans_et_plus_sans_fe,
                conf_mic_pv_preg = parasito_nombre_de_ge_fm_positifs_a_plasmodium_vivax_en_interne_fe,
                conf_mic_oth_u5 = parasito_nombre_de_ge_fm_positifs_a_autre_plasmodium_en_interne_moins_de_5_ans,
                conf_mic_oth_ov5 = parasito_nombre_de_ge_fm_positifs_a_autre_plasmodium_en_interne_5_ans_et_plus_sans_fe,
                conf_mic_oth_preg = parasito_nombre_de_ge_fm_positifs_a_autre_plasmodium_en_interne_fe,
                confall_u5 = c_ext_nombre_de_cas_de_paludisme_confirme_au_tdr_ge_simple_et_grave_moins_de_5_ans,
                confall_ov5 = c_ext_nombre_de_cas_de_paludisme_confirme_au_tdr_ge_simple_et_grave_5_ans_et_plus_sans_fe,
                confall_preg = c_ext_nombre_de_cas_de_paludisme_confirme_au_tdr_ge_simple_et_grave_fe,
                conf_u5 = c_ext_nombre_de_cas_de_paludisme_simple_confirme_au_tdr_ge_moins_de_5_ans,
                conf_ov5 = c_ext_nombre_de_cas_de_paludisme_simple_confirme_au_tdr_ge_5_ans_et_plus_sans_fe,
                conf_preg = c_ext_nombre_de_cas_de_paludisme_simple_confirme_au_tdr_ge_fe,
                confpres = palu_cas_de_paludisme_presume_et_confirmes,
                maltreat_u5_arte_lum = c_ext_nombre_de_patients_traites_avec_artemether_lumefantrine_moins_de_5_ans,
                maltreat_ov5_arte_lum = c_ext_nombre_de_patients_traites_avec_artemether_lumefantrine_5_ans_et_plus_sans_fe,
                maltreat_arte_lum_preg = c_ext_nombre_de_patients_traites_avec_artemether_lumefantrine_fe,
                maltreat_u5_artesu_amod = c_ext_nombre_de_patients_traites_avec_artesunate_amodiaquine_moins_de_5_ans,
                maltreat_ov5_artesu_amod = c_ext_nombre_de_patients_traites_avec_artesunate_amodiaquine_5_ans_et_plus_sans_fe,
                maltreat_artesu_amod_preg = c_ext_nombre_de_patients_traites_avec_artesunate_amodiaquine_fe,
                maltreat_u5_artesu_meflo = c_ext_nombre_de_patients_traites_avec_artesunate_mefloquine_moins_de_5_ans,
                maltreat_ov5_artesu_meflo = c_ext_nombre_de_patients_traites_avec_artesunate_mefloquine_5_ans_et_plus_sans_fe,
                maltreat_artesu_meflo_preg = c_ext_nombre_de_patients_traites_avec_artesunate_mefloquine_fe,
                maltreat_u5_dihydr_pip = c_ext_nombre_de_patients_traites_avec_dihydroartemisinine_piperaquine_moins_de_5_ans,
                maltreat_ov5_dihydr_pip = c_ext_nombre_de_patients_traites_avec_dihydroartemisinine_piperaquine_5_ans_et_plus_sans_fe,
                maltreat_dihydr_pip_preg = c_ext_nombre_de_patients_traites_avec_dihydroartemisinine_piperaquine_fe,
                maltreat_quinine_preg = c_ext_nombre_de_femmes_enceintes_traitees_avec_la_quinine,
                alladm_u5 = hospi_malades_nouvellement_admis_toutes_causes_moins_de_5_ans, 
                alladm_ov5 = hospi_malades_nouvellement_admis_toutes_causes_5_ans_et_plus_sans_fe,
                alladm_preg = hospi_malades_nouvellement_admis_toutes_causes_fe,
                maladm_u5_anemie= hospi_cause_de_paludisme_grave_forme_anemique_moins_de_5_ans,
                maladm_ov5_anemie = hospi_cause_de_paludisme_grave_forme_anemique_5_ans_et_plus_sans_fe,
                maladm_anemie_preg = hospi_cause_de_paludisme_grave_forme_anemique_fe, 
                maladm_u5_neuro = hospi_cause_de_paludisme_grave_forme_neurologique_moins_de_5_ans,
                maladm_ov5_neuro = hospi_cause_de_paludisme_grave_forme_neurologique_5_ans_et_plus_sans_fe,
                maladm_neuro_preg = hospi_cause_de_paludisme_grave_forme_neurologique_fe, 
                maladm_other_u5 = hospi_cause_de_paludisme_grave_autres_formes_moins_de_5_ans,
                maladm_other_ov5 = hospi_cause_de_paludisme_grave_autres_formes_5_ans_et_plus_sans_fe,
                maladm_other_preg = hospi_cause_de_paludisme_grave_autres_formes_fe, 
                maldth_u5 = hospi_cause_de_deces_paludisme_grave_toutes_formes_confondues_moins_de_5_ans,
                maldth_ov5 = hospi_cause_de_deces_paludisme_grave_toutes_formes_confondues_5_ans_et_plus_sans_fe,
                maldth_preg = hospi_cause_de_deces_paludisme_grave_toutes_formes_confondues_fe,
                alltdh_u5 = hospi_autres_causes_de_deces_toutes_confondues_moins_de_5_ans,
                alltdh_ov5 =hospi_autres_causes_de_deces_toutes_confondues_5_ans_et_plus_sans_fe,
                alldth_preg =hospi_autres_causes_de_deces_toutes_confondues_fe,
                allout_u5_chw = s_asc_nombre_total_de_malades_recus_au_niveau_communautaire_toutes_causes_moins_de_5_ans,
                allout_ov5_chw = s_asc_nombre_total_de_malades_recus_au_niveau_communautaire_toutes_causes_5_ans_et_plus_sans_fe,
                allout_preg_chw = s_asc_nombre_total_de_malades_recus_au_niveau_communautaire_toutes_causes_fe,
                susp_u5_chw = s_asc_nombre_de_cas_suspects_de_paludisme_au_niveau_communautaire_moins_de_5_ans,
                susp_ov5_chw = s_asc_nombre_de_cas_suspects_de_paludisme_au_niveau_communautaire_5_ans_et_plus_sans_fe,
                susp_preg_chw = s_asc_nombre_de_cas_suspects_de_paludisme_au_niveau_communautaire_fe,
                test_rdt_u5_chw = s_asc_nombre_de_cas_suspects_testes_avec_le_test_de_diagnostic_rapide_tdr_au_niveau_communautaire_moins_de_5_ans,
                test_rdt_ov5_chw = s_asc_nombre_de_cas_suspects_testes_avec_le_test_de_diagnostic_rapide_tdr_au_niveau_communautaire_5_ans_et_plus_sans_fe,
                test_rdt_preg_chw = s_asc_nombre_de_cas_suspects_testes_avec_le_test_de_diagnostic_rapide_tdr_au_niveau_communautaire_fe,
                conf_rdt_u5_chw = s_asc_nombre_de_cas_positifs_au_test_de_diagnostic_rapide_tdr_au_niveau_communautaire_moins_de_5_ans,
                conf_rdt_ov5_chw =s_asc_nombre_de_cas_positifs_au_test_de_diagnostic_rapide_tdr_au_niveau_communautaire_5_ans_et_plus_sans_fe,
                conf_rdt_preg_chw = s_asc_nombre_de_cas_positifs_au_test_de_diagnostic_rapide_tdr_au_niveau_communautaire_fe,
                maltreat_u5_chw = s_asc_nombre_de_cas_de_paludisme_confirme_traites_avec_cta_recommandes_au_niveau_communautaire_moins_de_5_ans,
                maltreat_ov5_chw = s_asc_nombre_de_cas_de_paludisme_confirme_traites_avec_cta_recommandes_au_niveau_communautaire_5_ans_et_plus_sans_fe,
                maltreat_preg_chw = s_asc_nombre_de_cas_de_paludisme_confirme_traites_avec_cta_recommandes_au_niveau_communautaire_fe,
                maltreat_u5_ACT_chw = s_asc_nombre_de_cas_de_paludisme_confirme_traites_avec_cta_recommandes_au_niveau_communautaire_moins_de_5_ans,
                maltreat_ov5_ACT_chw =s_asc_nombre_de_cas_de_paludisme_confirme_traites_avec_cta_recommandes_au_niveau_communautaire_5_ans_et_plus_sans_fe,
                maltreat_preg_ACT_chw =s_asc_nombre_de_cas_de_paludisme_confirme_traites_avec_cta_recommandes_au_niveau_communautaire_fe,
                anc1 = smi_nombre_de_femmes_vues_en_cpn1, llins_anc = smi_nombre_femmes_enceintes_ayant_recu_des_milda_lors_de_la_cpn,
                llins_epi = smi_nombre_enfants_de_0_11_mois_ayant_recu_des_milda, iptp1 = smi_nombre_de_femmes_enceintes_ayant_pris_une_dose_de_tpi,
                iptp2 = smi_nombre_de_femmes_enceintes_ayant_pris_deux_doses_de_tpi, iptp3 = smi_nombre_de_femmes_enceintes_ayant_pris_trois_doses_de_tpi,
                )%>%
  separate(period, into = c("month", "year"), sep = " ") %>% 
  mutate(month = case_when(month == "Janvier" ~ 1,
                           month == "Fevrier" ~ 2,
                           month == "Mars" ~ 3,
                           month == "Avril" ~ 4,
                           month == "Mai" ~ 5,
                           month == "Juin" ~ 6,
                           month == "Juillet" ~ 7,
                           month == "Aout" ~ 8,
                           month == "Septembre" ~ 9,
                           month == "Octobre" ~ 10,
                           month == "Novembre" ~ 11,
                           month == "Decembre" ~ 12
  )) %>% 
  mutate(month = as.numeric(month), year = as.numeric(year)) 

```

```
### Step 6: Compute new variables
```r
raw_data = raw_data %>%
  rowwise() %>% 
  mutate(allout = sum(allout_u5, allout_ov5, allout_preg,allout_u5_chw,allout_ov5_chw,allout_preg_chw, na.rm = TRUE),
         susp = sum(susp_u5, susp_ov5, susp_preg, susp_u5_chw, susp_ov5_chw, susp_preg_chw,na.rm = TRUE),
         test_u5 = sum(test_rdt_u5, test_mic_u5, test_rdt_u5_chw,na.rm = T),
         test_ov5 =sum(test_rdt_ov5, test_mic_ov5,test_rdt_ov5_chw, na.rm = T),
         test_preg = sum(test_rdt_preg, test_mic_preg,test_rdt_preg_chw, na.rm = T),
         #test_rdt = sum(test_rdt_u5, test_rdt_ov5, test_rdt_preg, na.rm = TRUE),
         #test_mic = sum(test_mic_u5, test_mic_ov5, test_mic_preg, na.rm = TRUE),
         test = sum(test_u5, test_ov5, test_preg, na.rm = TRUE),
         maltreat_u5 = sum(maltreat_u5_arte_lum, maltreat_u5_artesu_amod, maltreat_u5_artesu_meflo, maltreat_u5_dihydr_pip, maltreat_u5_chw,maltreat_u5_ACT_chw, na.rm=TRUE),
         maltreat_ov5 = sum(maltreat_ov5_arte_lum, maltreat_ov5_artesu_amod, maltreat_ov5_artesu_meflo, maltreat_ov5_dihydr_pip, maltreat_ov5_chw,maltreat_ov5_ACT_chw, na.rm=TRUE),
         maltreat_preg = sum(maltreat_dihydr_pip_preg, maltreat_arte_lum_preg, maltreat_artesu_meflo_preg,maltreat_artesu_amod_preg, maltreat_preg_chw,maltreat_preg_ACT_chw,na.rm = T),
         maltreat = sum(maltreat_u5, maltreat_ov5, maltreat_preg, na.rm=TRUE),
         conf_mic_u5 = sum(conf_mic_pf_u5, conf_mic_pm_u5, conf_mic_po_u5, conf_mic_pv_u5,conf_mic_oth_u5,na.rm=TRUE),
         conf_mic_ov5 = sum(conf_mic_pf_ov5, conf_mic_pm_ov5, conf_mic_po_ov5, conf_mic_pv_ov5,conf_mic_oth_ov5,na.rm=TRUE),
         conf_mic_preg = sum(conf_mic_pf_preg, conf_mic_pm_preg, conf_mic_po_preg, conf_mic_pv_preg, conf_mic_oth_preg, na.rm = T),
         conf_mic = sum(conf_mic_u5, conf_mic_ov5, conf_mic_preg, na.rm = T),
         #conf_rdt_mic_u5 = sum(conf_rdt_u5, conf_mic_u5, na.rm = TRUE),
         #conf_rdt_mic_ov5 = sum(conf_rdt_ov5, conf_mic_ov5, na.rm = TRUE),
         #conf_rdt_mic_preg = sum(conf_rdt_preg, conf_mic_preg, na.rm = T),
         #conf_rdt_mic = sum(conf_rdt_mic_u5, conf_rdt_mic_ov5, conf_rdt_mic_preg, na.rm = TRUE),
         conf_rdt = sum(conf_rdt_u5, conf_rdt_ov5,conf_rdt_preg, conf_rdt_u5_chw, conf_rdt_ov5_chw, conf_rdt_preg_chw, na.rm = T),
         conf = sum(conf_rdt, conf_mic, na.rm = TRUE),
         maladm_u5 = sum(maladm_u5_anemie, maladm_u5_neuro, maladm_other_u5, na.rm = TRUE),
         maladm_ov5 = sum(maladm_ov5_anemie, maladm_ov5_neuro, maladm_other_ov5, na.rm = TRUE),
         maladm_preg = sum(maladm_anemie_preg, maladm_neuro_preg, maladm_other_preg, na.rm = T),
         maladm = sum(maladm_u5, maladm_ov5, maladm_preg, na.rm = TRUE),
         alladm = sum(alladm_u5, alladm_ov5,alladm_preg, na.rm = TRUE),
         maldth = sum(maldth_u5, maldth_ov5, maldth_preg, na.rm = TRUE),
         alldth = sum(alltdh_u5, alldth_preg, alltdh_ov5, na.rm = T),
         susp_chw = sum(susp_u5_chw, susp_ov5_chw,susp_preg_chw, na.rm = TRUE),
         test_rdt_chw = sum(test_rdt_u5_chw, test_rdt_ov5_chw, test_rdt_preg_chw, na.rm = TRUE),
         conf_rdt_chw = sum(conf_rdt_u5_chw, conf_rdt_ov5_chw, conf_rdt_preg_chw, na.rm = TRUE),
         maltreat_chw = sum(maltreat_u5_chw, maltreat_ov5_chw,maltreat_preg_chw, na.rm = TRUE),
         confsev_u5 = maladm_u5, confsev_ov5 = maladm_ov5, confsev = maladm,
         iptp1 = sum(iptp1, na.rm = T),
         iptp2 = sum(iptp2, na.rm = T), iptp3 = sum(iptp3, na.rm = T)) 


```
### Step 7: Transform data element into appropriate data type
```r

raw_data = raw_data %>%

### Step 8: Save the transform data in excel/csv
```r

write.csv(raw_data, 'cleaning_data.csv')

```
### Step 9: Aggregate data element monthly and yearly at the appropriate admnin level


#### Step 9.1: Aggreate data element monthly at the appropriate admnin level and save it
```r
monthly_data_DS = raw_data %>%
  group_by(adm1, adm2, year, month) %>%
  dplyr::summarise(across(allout_u5:llins_stockout_days, ~ sum(.x, na.rm = TRUE)))

write.csv(monthly_data_DS, 'Monthly_HD_data.csv')
```
#### Step 9.2: Aggreate data element yearly at the appropriate admnin level and save it
```r
yearly_data_DS = raw_data %>%
  group_by(adm1, adm2, year) %>%
  dplyr::summarise(across(allout_u5:llins_stockout_days, ~ sum(.x, na.rm = TRUE)))

write.csv(yearly_data_DS, 'Yearly_HD_data.csv')
```


