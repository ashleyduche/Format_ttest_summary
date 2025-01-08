# Function to format t-test results from long-format data
format_ttest_summary <- function(data, col_name, value_col, comparisons) {
  # Initialize an empty data frame to store results
  results_df <- data.frame(
    Comparison_Groups = character(),
    M = character(),
    SD = character(),
    df = numeric(),
    t = numeric(),
    p = character(),
    p_value = character(),
    Lower = numeric(),
    Upper = numeric(),
    stringsAsFactors = FALSE
  )
  
  # Loop through each comparison
  for (comparison in comparisons) {
    group1 <- comparison$group1
    group2 <- comparison$group2
    
    # Filter data for each group
    data_group1 <- data[[value_col]][data[[col_name]] == group1]
    data_group2 <- data[[value_col]][data[[col_name]] == group2]
    
    # Perform t-test
    ttest_result <- t.test(data_group1, data_group2)
    
    # Calculate means and standard deviations with 3 significant figures
    mean1 <- format(signif(mean(data_group1, na.rm = TRUE), 3), digits = 3, scientific = FALSE)
    sd1 <- format(signif(sd(data_group1, na.rm = TRUE), 3), digits = 3, scientific = FALSE)
    mean2 <- format(signif(mean(data_group2, na.rm = TRUE), 3), digits = 3, scientific = FALSE)
    sd2 <- format(signif(sd(data_group2, na.rm = TRUE), 3), digits = 3, scientific = FALSE)
    
    # Create formatted p-value
    p_value <- format(signif(ttest_result$p.value, 4), scientific = TRUE)
    formatted_p <- if (ttest_result$p.value < 0.001) "p < 0.001" else paste("p =", signif(ttest_result$p.value, 3))
    
    # Append the t-test results
    results_df <- rbind(
      results_df,
      data.frame(
        Comparison_Groups = paste(group1, "vs", group2),
        M = paste(mean1, "/", mean2),
        SD = paste(sd1, "/", sd2),
        df = signif(ttest_result$parameter, 3),
        t = signif(ttest_result$statistic, 3),
        p = formatted_p,
        p_value = p_value,
        Lower = signif(ttest_result$conf.int[1], 3),
        Upper = signif(ttest_result$conf.int[2], 3),
        stringsAsFactors = FALSE
      )
    )
  }
  
  # Return the formatted results
  return(results_df)
}

# Example usage
# Example long-format data
example_data <- data.frame(
  prediction = c(rnorm(10, mean = 5, sd = 1), rnorm(10, mean = 7, sd = 1.5)),
  meta_group_A = c(rep("control", 10), rep("experimental", 10))
)

# Define comparisons (list format)
example_comparisons <- list(
  list(group1 = "control", group2 = "experimental")
)

# Run the function
example_results <- format_ttest_summary(
  data = example_data,
  col_name = "meta_group_A",
  value_col = "prediction",
  comparisons = example_comparisons
)

# Print example results
print(example_results)
