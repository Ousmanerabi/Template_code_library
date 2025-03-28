# Step 1: Install and load required libraries
if (!requireNamespace("readxl", quietly = TRUE)) {
  install.packages("readxl")
}
if (!requireNamespace("writexl", quietly = TRUE)) {
  install.packages("writexl")
}

library(readxl)
library(writexl)
library(httr)

# Step 2: Define a function to read and validate Excel files from GitHub
read_excel_file <- function(url) {
  tryCatch({
    # Download the file temporarily
    temp_file <- tempfile(fileext = ".xls")
    GET(url, write_disk(temp_file, overwrite = TRUE))

    # Read the Excel file
    message(paste("Reading file:", url))
    df <- read_excel(temp_file, sheet = 1)
    return(df)
  }, error = function(e) {
    message(paste("Error reading file:", url, "-", e$message))
    return(NULL)
  })
}

# Step 3: Combine Excel files with validation
combine_excel_files_with_validation <- function(file_urls, output_file) {
  tryCatch({
    # Read the reference file
    reference_df <- read_excel_file(file_urls[1])
    if (is.null(reference_df)) {
      stop("Reference file could not be read.")
    }

    # Get reference column names and count
    reference_columns <- colnames(reference_df)
    reference_column_length <- length(reference_columns)

    # Initialize list to store DataFrames
    combined_data <- list()
    skipped_files <- list()

    # Iterate through URLs
    for (url in file_urls) {
      df <- read_excel_file(url)
      if (is.null(df)) {
        skipped_files <- append(skipped_files, url)
        next
      }

      # Validate column names and count
      if (!identical(colnames(df), reference_columns)) {
        message(paste("Skipping file due to column mismatch:", url))
        skipped_files <- append(skipped_files, url)
        next
      }
      if (ncol(df) != reference_column_length) {
        message(paste("Skipping file due to column count mismatch:", url))
        skipped_files <- append(skipped_files, url)
        next
      }

      # Add the valid DataFrame to the list
      combined_data <- append(combined_data, list(df))
    }

    # Combine all valid DataFrames
    if (length(combined_data) == 0) {
      stop("No valid files to combine.")
    }
    combined_df <- do.call(rbind, combined_data)

    # Save the combined data to an Excel file
    write_xlsx(combined_df, output_file)
    message(paste("Files combined successfully! Output saved to", output_file))

    # Log skipped files
    if (length(skipped_files) > 0) {
      message("The following files were skipped:")
      print(skipped_files)
    }
  }, error = function(e) {
    message(paste("Error:", e$message))
  })
}

# Step 4: Define the GitHub raw file URLs
base_url <- "https://github.com/ahadi-analytics/snt-data-files/tree/11e06559007c729d65bf9849097e6bc90379202d/Excel%20files/Routine%20data"


file_names <- c(
  "Bo_District_2015-2023.xls",
  "Bombali_District_2015-2023.xls",
  "Bonthe_District_2015-2023.xls",
  "Falaba_District_2015-2023.xls",
  "Kailahun_District_2015-2023.xls",
  "Kambia_District_2015-2023.xls",
  "Karene_District_2015-2023.xls",
  "Kenema_District_2015-2023.xls",
  "Moyamba_District_2015-2023.xls",
  "Kono_District_2015-2023.xls",
  "Port_Loko_District_2015-2023.xls",
  "Pujehun_District_2015-2023.xls",
  "Tonkolili_District_2015-2023.xls",
  "Western_Area_Rural_District_2015-2023.xls",
  "Western_Area_Urban_District_2015-2023.xls",
  "Koinadugu_District_2015-2023.xls"
)

file_urls <- paste0(base_url, file_names)

# Step 5: Call the function to combine files
output_file <- "malaria_routine_data.xlsx"
combine_excel_files_with_validation(file_urls, output_file)


# Step 1: Install and load required library
if (!requireNamespace("readxl", quietly = TRUE)) {
  install.packages("readxl")
}

library(readxl)

# Step 2: Define the function to read the Excel file
read_combined_excel <- function(file_path) {
  tryCatch({
    # Read the first sheet of the Excel file
    combined_df <- read_excel(file_path, sheet = 1)
    message("Combined DataFrame loaded successfully!")
    return(combined_df)
  }, error = function(e) {
    if (grepl("cannot open file", e$message)) {
      message(paste("Error: The file '", file_path, "' was not found.", sep = ""))
    } else {
      message(paste("Error reading the Excel file:", e$message))
    }
    return(NULL)
  })
}

# Step 3: Call the function
output_file <- "malaria_routine_data.xlsx"  # Specify the path to the Excel file
combined_data <- read_combined_excel(output_file)

# Step 4: Display the first few rows if the data was loaded successfully
if (!is.null(combined_data)) {
  print(head(combined_data))  # Display the first few rows
}

# Step 5: Create a copy of the data (if needed)
df <- combined_data


install.packages("stringr")
library(stringr)

process_dataframe_with_integer_month <- function(df, column_to_split, drop_column) {
  # Create month mapping dictionary
  month_map <- c(
    "January" = "01", "February" = "02", "March" = "03", "April" = "04",
    "May" = "05", "June" = "06", "July" = "07", "August" = "08",
    "September" = "09", "October" = "10", "November" = "11", "December" = "12"
  )

  tryCatch({
    # Split the column into month and year
    split_cols <- stringr::str_split_fixed(df[[column_to_split]], " ", 2)
    df$month <- split_cols[, 1]
    df$year <- split_cols[, 2]

    # Map month names to numbers
    df$month <- month_map[df$month]

    # Convert year to numeric
    df$year <- as.numeric(df$year)

    # Create Date column in YYYY-MM format
    df$Date <- paste(df$year, df$month, sep = "-")

    # Drop specified columns
    df <- df[, !names(df) %in% c(column_to_split, drop_column)]

    return(df)
  }, error = function(e) {
    message(paste("Error processing DataFrame:", e))
    return(NULL)
  })
}

# Call the function
df <- process_dataframe_with_integer_month(df, "periodname", "orgunitlevel5")


rename_columns <- function(df) {
 tryCatch({
   # Define mapping dictionaries
   orgunit_rename <- c(
     orgunitlevel1 = "adm0",
     orgunitlevel2 = "adm1",
     orgunitlevel3 = "adm2",
     orgunitlevel4 = "adm3",
     organisationunitname = "hf"
   )

   column_rename <- c(
     "OPD (New and follow-up curative) 0-59m_X" = "allout_u5",
     "OPD (New and follow-up curative) 5+y_X" = "allout_ov5",

     "Admission - Child with malaria 0-59 months_X" = "maladm_u5",
     "Admission - Child with malaria 5-14 years_X" = "maladm_5_14",
     "Admission - Malaria 15+ years_X" = "maladm_ov15",

     "Child death - Malaria 1-59m_X" = "maldth_1_59m",
     "Child death - Malaria 10-14y_X" = "maldth_10_14",
     "Child death - Malaria 5-9y_X" = "maldth_5_9",

     "Death malaria 15+ years Female" = "maldth_fem_ov15",
     "Death malaria 15+ years Male" = "maldth_mal_ov15",

     "Separation - Child with malaria 0-59 months_X Death" = "maldth_u5",
     "Separation - Child with malaria 5-14 years_X Death" = "maldth_5_14",
     "Separation - Malaria 15+ years_X Death" = "maldth_ov15",

     "Fever case - suspected Malaria 0-59m_X" = "susp_u5_hf",
     "Fever case - suspected Malaria 5-14y_X" = "susp_5_14_hf",
     "Fever case - suspected Malaria 15+y_X" = "susp_ov15_hf",
     "Fever case in community (Suspected Malaria) 0-59m_X" = "susp_u5_com",
     "Fever case in community (Suspected Malaria) 5-14y_X" = "susp_5_14_com",
     "Fever case in community (Suspected Malaria) 15+y_X" = "susp_ov15_com",

     "Fever case in community tested for Malaria (RDT) - Negative 0-59m_X" = "tes_neg_rdt_u5_com",
     "Fever case in community tested for Malaria (RDT) - Positive 0-59m_X" = "tes_pos_rdt_u5_com",
     "Fever case in community tested for Malaria (RDT) - Negative 5-14y_X" = "tes_neg_rdt_5_14_com",
     "Fever case in community tested for Malaria (RDT) - Positive 5-14y_X" = "tes_pos_rdt_5_14_com",
     "Fever case in community tested for Malaria (RDT) - Negative 15+y_X" = "tes_neg_rdt_ov15_com",
     "Fever case in community tested for Malaria (RDT) - Positive 15+y_X" = "tes_pos_rdt_ov15_com",
     "Fever case tested for Malaria (Microscopy) - Negative 0-59m_X" = "test_neg_mic_u5_hf",
     "Fever case tested for Malaria (Microscopy) - Positive 0-59m_X" = "test_pos_mic_u5_hf",
     "Fever case tested for Malaria (Microscopy) - Negative 5-14y_X" = "test_neg_mic_5_14_hf",
     "Fever case tested for Malaria (Microscopy) - Positive 5-14y_X" = "test_pos_mic_5_14_hf",
     "Fever case tested for Malaria (Microscopy) - Negative 15+y_X" = "test_neg_mic_ov15_hf",
     "Fever case tested for Malaria (Microscopy) - Positive 15+y_X" = "test_pos_mic_ov15_hf",
     "Fever case tested for Malaria (RDT) - Negative 0-59m_X" = "tes_neg_rdt_u5_hf",
     "Fever case tested for Malaria (RDT) - Positive 0-59m_X" = "tes_pos_rdt_u5_hf",
     "Fever case tested for Malaria (RDT) - Negative 5-14y_X" = "tes_neg_rdt_5_14_hf",
     "Fever case tested for Malaria (RDT) - Positive 5-14y_X" = "tes_pos_rdt_5_14_hf",
     "Fever case tested for Malaria (RDT) - Negative 15+y_X" = "tes_neg_rdt_ov15_hf",
     "Fever case tested for Malaria (RDT) - Positive 15+y_X" = "tes_pos_rdt_ov15_hf",

     "Malaria treated in community with ACT <24 hours 0-59m_X" = "maltreat_u24_u5_com",
     "Malaria treated in community with ACT >24 hours 0-59m_X" = "maltreat_ov24_u5_com",
     "Malaria treated in community with ACT <24 hours 5-14y_X" = "maltreat_u24_5_14_com",
     "Malaria treated in community with ACT >24 hours 5-14y_X" = "maltreat_ov24_5_14_com",
     "Malaria treated in community with ACT <24 hours 15+y_X" = "maltreat_u24_ov15_com",
     "Malaria treated in community with ACT >24 hours 15+y_X" = "maltreat_ov24_ov15_com",
     "Malaria treated with ACT <24 hours 0-59m_X" = "maltreat_u24_u5_hf",
     "Malaria treated with ACT >24 hours 0-59m_X" = "maltreat_ov24_u5_hf",
     "Malaria treated with ACT <24 hours 5-14y_X" = "maltreat_u24_5_14_hf",
     "Malaria treated with ACT >24 hours 5-14y_X" = "maltreat_ov24_5_14_hf",
     "Malaria treated with ACT <24 hours 15+y_X" = "maltreat_u24_ov15_hf",
     "Malaria treated with ACT >24 hours 15+y_X" = "maltreat_ov24_ov15_hf"
   )

   # Rename using both mappings
   names(df) <- dplyr::recode(names(df), !!!c(orgunit_rename, column_rename))

   return(df)
 }, error = function(e) {
   message(paste("Error renaming columns:", e))
   return(NULL)
 })
}

# Call function
df <- rename_columns(df)


create_variables <- function(df) {
 tryCatch({
   # allout
   df$allout <- rowSums(df[c("allout_u5", "allout_ov5")], na.rm = TRUE)

   # susp
   df$susp <- rowSums(df[c("susp_u5_hf", "susp_5_14_hf", "susp_ov15_hf",
                          "susp_u5_com", "susp_5_14_com", "susp_ov15_com")], na.rm = TRUE)

   # test_hf
   test_hf_cols <- c("test_neg_mic_u5_hf", "test_pos_mic_u5_hf", "test_neg_mic_5_14_hf",
                     "test_pos_mic_5_14_hf", "test_neg_mic_ov15_hf", "test_pos_mic_ov15_hf",
                     "tes_neg_rdt_u5_hf", "tes_pos_rdt_u5_hf", "tes_neg_rdt_5_14_hf",
                     "tes_pos_rdt_5_14_hf", "tes_neg_rdt_ov15_hf", "tes_pos_rdt_ov15_hf")
   df$test_hf <- rowSums(df[test_hf_cols], na.rm = TRUE)

   # test_com
   test_com_cols <- c("tes_neg_rdt_u5_com", "tes_pos_rdt_u5_com", "tes_neg_rdt_5_14_com",
                      "tes_pos_rdt_5_14_com", "tes_neg_rdt_ov15_com", "tes_pos_rdt_ov15_com")
   df$test_com <- rowSums(df[test_com_cols], na.rm = TRUE)

   # test
   df$test <- rowSums(df[c("test_hf", "test_com")], na.rm = TRUE)

   # conf_hf
   conf_hf_cols <- c("test_pos_mic_u5_hf", "test_pos_mic_5_14_hf", "test_pos_mic_ov15_hf",
                     "tes_pos_rdt_u5_hf", "tes_pos_rdt_5_14_hf", "tes_pos_rdt_ov15_hf")
   df$conf_hf <- rowSums(df[conf_hf_cols], na.rm = TRUE)

   # conf_com
   conf_com_cols <- c("tes_pos_rdt_u5_com", "tes_pos_rdt_5_14_com", "tes_pos_rdt_ov15_com")
   df$conf_com <- rowSums(df[conf_com_cols], na.rm = TRUE)

   # conf
   df$conf <- rowSums(df[c("conf_hf", "conf_com")], na.rm = TRUE)

   # maltreat_com
   maltreat_com_cols <- c("maltreat_u24_u5_com", "maltreat_ov24_u5_com", "maltreat_u24_5_14_com",
                         "maltreat_ov24_5_14_com", "maltreat_u24_ov15_com", "maltreat_ov24_ov15_com")
   df$maltreat_com <- rowSums(df[maltreat_com_cols], na.rm = TRUE)

   # maltreat_hf
   maltreat_hf_cols <- c("maltreat_u24_u5_hf", "maltreat_ov24_u5_hf", "maltreat_u24_5_14_hf",
                        "maltreat_ov24_5_14_hf", "maltreat_u24_ov15_hf", "maltreat_ov24_ov15_hf")
   df$maltreat_hf <- rowSums(df[maltreat_hf_cols], na.rm = TRUE)

   # maltreat
   df$maltreat <- rowSums(df[c("maltreat_hf", "maltreat_com")], na.rm = TRUE)

   # pres_com
   df$pres_com <- pmax(df$maltreat_com - df$conf_com, 0, na.rm = TRUE)

   # pres_hf
   df$pres_hf <- pmax(df$maltreat_hf - df$conf_hf, 0, na.rm = TRUE)

   # pres
   df$pres <- rowSums(df[c("pres_com", "pres_hf")], na.rm = TRUE)

   # maladm
   df$maladm <- rowSums(df[c("maladm_u5", "maladm_5_14", "maladm_ov15")], na.rm = TRUE)

   # maldth
   maldth_cols <- c("maldth_u5", "maldth_1_59m", "maldth_10_14", "maldth_5_9",
                    "maldth_5_14", "maldth_ov15", "maldth_fem_ov15", "maldth_mal_ov15")
   df$maldth <- rowSums(df[maldth_cols], na.rm = TRUE)

   return(df)
 }, error = function(e) {
   message(paste("Error creating variables:", e))
   return(NULL)
 })
}

df <- create_variables(df)


create_hfid_column <- function(df) {
 df <- df %>%
   group_by(adm1, adm2, adm3, hf) %>%
   mutate(hf_uid = sprintf("hf_%04d", cur_group_id())) %>%
   ungroup()
 return(df)
}

# Load required package
library(dplyr)

# Call function
df <- create_hfid_column(df)

install.packages(c("dplyr", "writexl", "zoo"))
library(dplyr)
library(writexl)
library(zoo)

# Helper functions
detect_outliers_scatterplot <- function(df, col) {
 Q1 <- quantile(df[[col]], 0.25)
 Q3 <- quantile(df[[col]], 0.75)
 IQR <- Q3 - Q1
 lower_bound <- Q1 - 1.5 * IQR
 upper_bound <- Q3 + 1.5 * IQR
 return(list(lower=lower_bound, upper=upper_bound))
}

calculate_moving_avg <- function(series, window) {
 zoo::rollmean(series, k=window, fill=NA, align="right")
}

calculate_moving_avg_excluding_outliers <- function(series, window, threshold=1.5) {
 q1 <- quantile(series, 0.25, na.rm=TRUE)
 q3 <- quantile(series, 0.75, na.rm=TRUE)
 iqr <- q3 - q1
 lower_bound <- q1 - threshold * iqr
 upper_bound <- q3 + threshold * iqr

 clean_series <- ifelse(series >= lower_bound & series <= upper_bound, series, NA)
 ma <- zoo::rollmean(clean_series, k=window, fill=NA, align="right")
 ma <- zoo::na.locf(ma, na.rm=FALSE)
 ma <- zoo::na.locf(ma, fromLast=TRUE)

 return(ma)
}

process_column_export <- function(df, column, output_file) {
 results <- df %>%
   group_by(adm1, adm2, adm3, hf, year) %>%
   group_modify(~{
     bounds <- detect_outliers_scatterplot(.x, column)

     .x[[paste0(column, "_lower_bound")]] <- bounds$lower
     .x[[paste0(column, "_upper_bound")]] <- bounds$upper
     .x[[paste0(column, "_category")]] <- ifelse(
       .x[[column]] < bounds$lower | .x[[column]] > bounds$upper,
       "Outlier", "Non-Outlier"
     )

     non_outliers <- .x[[column]] >= bounds$lower & .x[[column]] <= bounds$upper

     mean_include <- mean(.x[[column]], na.rm=TRUE)
     mean_exclude <- mean(.x[[column]][non_outliers], na.rm=TRUE)
     median_include <- median(.x[[column]], na.rm=TRUE)
     median_exclude <- median(.x[[column]][non_outliers], na.rm=TRUE)
     moving_avg_include <- calculate_moving_avg(.x[[column]], 3)
     moving_avg_exclude <- calculate_moving_avg_excluding_outliers(.x[[column]], 3)
     winsorised <- pmin(pmax(.x[[column]], bounds$lower), bounds$upper)

     .x[[paste0(column, "_corrected_mean_include")]] <- ifelse(.x[[paste0(column, "_category")]] == "Outlier", mean_include, .x[[column]])
     .x[[paste0(column, "_corrected_mean_exclude")]] <- ifelse(.x[[paste0(column, "_category")]] == "Outlier", mean_exclude, .x[[column]])
     .x[[paste0(column, "_corrected_median_include")]] <- ifelse(.x[[paste0(column, "_category")]] == "Outlier", median_include, .x[[column]])
     .x[[paste0(column, "_corrected_median_exclude")]] <- ifelse(.x[[paste0(column, "_category")]] == "Outlier", median_exclude, .x[[column]])
     .x[[paste0(column, "_corrected_moving_avg_include")]] <- ifelse(.x[[paste0(column, "_category")]] == "Outlier", moving_avg_include, .x[[column]])
     .x[[paste0(column, "_corrected_moving_avg_exclude")]] <- ifelse(.x[[paste0(column, "_category")]] == "Outlier", moving_avg_exclude, .x[[column]])
     .x[[paste0(column, "_corrected_winsorised")]] <- ifelse(.x[[paste0(column, "_category")]] == "Outlier", winsorised, .x[[column]])

     return(.x)
   }) %>%
   ungroup()

 export_columns <- c(
   "adm1", "adm2", "adm3", "hf", "hf_uid", "year", "month", column,
   paste0(column, c("_category", "_lower_bound", "_upper_bound",
                   "_corrected_mean_include", "_corrected_mean_exclude",
                   "_corrected_median_include", "_corrected_median_exclude",
                   "_corrected_moving_avg_include", "_corrected_moving_avg_exclude",
                   "_corrected_winsorised"))
 )

 write_xlsx(results[export_columns], output_file)
}

columns_to_process <- c("allout", "susp", "test", "conf", "maltreat", "pres", "maladm", "maldth")

for(column in columns_to_process) {
 output_file <- paste0(column, "_results.xlsx")
 process_column_export(df, column, output_file)
}
