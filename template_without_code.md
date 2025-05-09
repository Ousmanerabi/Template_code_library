### Full code

```r
install.packages(c('dplyr', 'readxl', 'tidyverse', 'janitor', 'writexl'))

library(dplyr)
library(readxl)
library(tidyverse)
library(janitor)
library(writexl)

setwd("path/to/your/directory)

raw_data = read_excel('file path/filename.xlsx')
data = list.files(pattern = ".xls", full.names = TRUE)

myfiles = lapply(data, read_xls)
raw_data <- dplyr::bind_rows(myfiles)

head()
str()
glimpse()

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


detect_outliers <- function(df, column) {
df_stats_mean <- raw_data %>%
    group_by(adm1, adm2, hf, UID, year) %>% 
    summarise(
      moyenne = ceiling(mean(.data[[column]], na.rm = TRUE)),
      sd = ceiling(sd(.data[[column]], na.rm = TRUE)),
      lower_bound = moyenne - 3 * sd,
      upper_bound = moyenne + 3 * sd,
      .groups = "drop"
    )

  df_result <- df %>%
    left_join(df_stats, by = c("adm1", "adm2", "hf", "UID", "year")) %>%
    mutate(
      outliers_moyenne = ifelse(.data[[column]] < lower_bound | .data[[column]] > upper_bound, "outliers", "normales values")
)
return(df_result)
}

detect_outliers <- function(df, column) {
df_stats_iqr <- raw_data %>%
    group_by(adm1, adm2, hf, UID, year) %>%  # Removed Variables if it's not needed for grouping
    arrange(adm1, adm2, hf, UID,year)%>%
    summarise(
      Q1 = quantile(.data[[column]], 0.25, na.rm = TRUE),
      Q3 = quantile(.data[[column]], 0.75, na.rm = TRUE),
      IQR = quantile(.data[[column]], 0.75, na.rm = TRUE) - quantile(.data[[column]], 0.25, na.rm = TRUE),
      lower_bound_iqr = quantile(.data[[column]], 0.25, na.rm = TRUE) - 1.5 * (quantile(.data[[column]], 0.75, na.rm = TRUE) - quantile(.data[[column]], 0.25, na.rm = TRUE)),
      upper_bound_iqr = quantile(.data[[column]], 0.75, na.rm = TRUE) + 1.5 * (quantile(.data[[column]], 0.75, na.rm = TRUE) - quantile(.data[[column]], 0.25, na.rm = TRUE)),
      .groups = "drop"
    )

  df_result <- df %>%
    left_join(df_stats, by = c("adm1", "adm2", "hf", "UID", "year")) %>%
    mutate(
      outliers_IQR = ifelse(.data[[column]] < lower_bound_iqr | .data[[column]] > upper_bound_iqr, "outliers", "normales values")
)
return(df_result)
}

detect_outliers <- function(df, column) {
  df_stats_mad <- raw_data %>%
    group_by(adm1, adm2, hf, UID, year) %>%  # Removed Variables if it's not needed for grouping
    summarise(
      mediane = ceiling(median(.data[[column]], na.rm = TRUE)),
      median_absolute = ceiling(mad(.data[[column]], constant = 1, na.rm = TRUE)),
      BI_median = mediane - 15 * median_absolute,
      BS_median = mediane + 15 * median_absolute,
      .groups = "drop"
    )

  df_result <- df %>%
    left_join(df_stats, by = c("adm1", "adm2", "hf", "UID", "year")) %>%
    mutate(
      outliers_mad = ifelse(.data[[column]] < BI_median | .data[[column]] > BS_median, "outliers", "normales values")
)
return(df_result)
}

find_valid_neighbors <- function(i, values) {
  
  last_valid <- NA
  for (j in (i - 1):1) {
    if (!is.na(values[j]) && values[j] != 0) {
      last_valid <- values[j]
      break
    }
  }
  
  next_valid <- NA
  for (j in (i + 1):length(values)) {
    if (!is.na(values[j]) && values[j] != 0) {
      next_valid <- values[j]
      break
    }
  }

  if (is.na(last_valid) || is.na(next_valid)) {
    return(NA)
  } else {
    return(mean(c(last_valid, next_valid)))
  }
}


ind= which(df_results$outliers_halper == "outliers")


df_results$imput_values = rep(NA, length(nrow(df_results)))


df_results$imput_values[ind] <- sapply(ind, function(i) find_valid_neighbors(i, df_results$values))


data_compute = df_results %>%
  rowwise() %>%
  mutate(
   
    allout = sum(across(c(allout_u5, allout_ov5, allout_preg, 
                         allout_u5_chw, allout_ov5_chw, allout_preg_chw), 
                        sum, na.rm = TRUE)),
    
 
    susp = sum(across(c(susp_u5, susp_ov5, susp_preg, 
                       susp_u5_chw, susp_ov5_chw, susp_preg_chw), 
                      sum, na.rm = TRUE)),
    susp_chw = sum(across(c(susp_u5_chw, susp_ov5_chw, susp_preg_chw), 
                         sum, na.rm = TRUE)),
    
   
    test_u5 = sum(across(c(test_rdt_u5, test_mic_u5, test_rdt_u5_chw), 
                        sum, na.rm = TRUE)),
    test_ov5 = sum(across(c(test_rdt_ov5, test_mic_ov5, test_rdt_ov5_chw), 
                         sum, na.rm = TRUE)),
    test_preg = sum(across(c(test_rdt_preg, test_mic_preg, test_rdt_preg_chw), 
                          sum, na.rm = TRUE)),
    test = sum(test_u5, test_ov5, test_preg, na.rm = TRUE),
    test_rdt_chw = sum(across(c(test_rdt_u5_chw, test_rdt_ov5_chw, test_rdt_preg_chw), 
                             sum, na.rm = TRUE)),
    

    maltreat_u5 = sum(across(c(maltreat_u5_arte_lum, maltreat_u5_artesu_amod, 
                             maltreat_u5_artesu_meflo, maltreat_u5_dihydr_pip, 
                             maltreat_u5_chw, maltreat_u5_ACT_chw), 
                            sum, na.rm = TRUE)),
    maltreat_ov5 = sum(across(c(maltreat_ov5_arte_lum, maltreat_ov5_artesu_amod, 
                              maltreat_ov5_artesu_meflo, maltreat_ov5_dihydr_pip, 
                              maltreat_ov5_chw, maltreat_ov5_ACT_chw), 
                             sum, na.rm = TRUE)),
    maltreat_preg = sum(across(c(maltreat_dihydr_pip_preg, maltreat_arte_lum_preg, 
                               maltreat_artesu_meflo_preg, maltreat_artesu_amod_preg, 
                               maltreat_preg_chw, maltreat_preg_ACT_chw), 
                              sum, na.rm = TRUE)),
    maltreat = sum(maltreat_u5, maltreat_ov5, maltreat_preg, na.rm = TRUE),
    maltreat_chw = sum(across(c(maltreat_u5_chw, maltreat_ov5_chw, maltreat_preg_chw), 
                             sum, na.rm = TRUE)),
    
   
    conf_mic_u5 = sum(across(c(conf_mic_pf_u5, conf_mic_pm_u5, conf_mic_po_u5, 
                             conf_mic_pv_u5, conf_mic_oth_u5), 
                            sum, na.rm = TRUE)),
    conf_mic_ov5 = sum(across(c(conf_mic_pf_ov5, conf_mic_pm_ov5, conf_mic_po_ov5, 
                              conf_mic_pv_ov5, conf_mic_oth_ov5), 
                             sum, na.rm = TRUE)),
    conf_mic_preg = sum(across(c(conf_mic_pf_preg, conf_mic_pm_preg, conf_mic_po_preg, 
                               conf_mic_pv_preg, conf_mic_oth_preg), 
                              sum, na.rm = TRUE)),
    conf_mic = sum(conf_mic_u5, conf_mic_ov5, conf_mic_preg, na.rm = TRUE),
    
    # Confirmed RDT and total confirmed
    conf_rdt = sum(across(c(conf_rdt_u5, conf_rdt_ov5, conf_rdt_preg, 
                          conf_rdt_u5_chw, conf_rdt_ov5_chw, conf_rdt_preg_chw), 
                         sum, na.rm = TRUE)),
    conf_rdt_chw = sum(across(c(conf_rdt_u5_chw, conf_rdt_ov5_chw, conf_rdt_preg_chw), 
                             sum, na.rm = TRUE)),
    conf = sum(conf_rdt, conf_mic, na.rm = TRUE),
    
  
    maladm_u5 = sum(across(c(maladm_u5_anemie, maladm_u5_neuro, maladm_other_u5), 
                          sum, na.rm = TRUE)),
    maladm_ov5 = sum(across(c(maladm_ov5_anemie, maladm_ov5_neuro, maladm_other_ov5), 
                           sum, na.rm = TRUE)),
    maladm_preg = sum(across(c(maladm_anemie_preg, maladm_neuro_preg, maladm_other_preg), 
                            sum, na.rm = TRUE)),
    maladm = sum(maladm_u5, maladm_ov5, maladm_preg, na.rm = TRUE),
    
    # All admissions and deaths
    alladm = sum(across(c(alladm_u5, alladm_ov5, alladm_preg), 
                       sum, na.rm = TRUE)),
    maldth = sum(across(c(maldth_u5, maldth_ov5, maldth_preg), 
                       sum, na.rm = TRUE)),
    alldth = sum(across(c(alltdh_u5, alldth_preg, alltdh_ov5), 
                       sum, na.rm = TRUE)),
    
    confsev_u5 = maladm_u5,
    confsev_ov5 = maladm_ov5,
    confsev = maladm,
    iptp1 = sum(iptp1, na.rm = TRUE),
    iptp2 = sum(iptp2, na.rm = TRUE),
    iptp3 = sum(iptp3, na.rm = TRUE)
  )

write.csv(df_results, 'HF_level_cleaning_data.csv')
monthly_data_DS = df_results %>%
  group_by(adm1, adm2, year, month) %>%
  dplyr::summarise(across(allout_u5:llins_stockout_days, ~ sum(.x, na.rm = TRUE)))

write.csv(monthly_data_DS, 'Monthly_HD_data.csv')

yearly_data_DS = df_results %>%
  group_by(adm1, adm2, year) %>%
  dplyr::summarise(across(allout_u5:llins_stockout_days, ~ sum(.x, na.rm = TRUE)))

write.csv(yearly_data_DS, 'Yearly_HD_data.csv')

```


