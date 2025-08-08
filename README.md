# Course Effectiveness Analysis

This repository contains a Python notebook that demonstrates the process of generating, cleaning, and clustering a synthetic dataset of course enrollments. The analysis focuses on identifying groups of courses based on their characteristics and evaluating these clusters based on completion rates.

## Project Structure

- `course_effectiveness.csv`: The initial synthetic dataset with introduced data quality issues.
- `cleaned_course_effectiveness.csv`: The cleaned dataset after handling missing values, outliers, and standardizing/encoding categorical variables.
- `course_enrollments.db`: An SQLite database containing the cleaned course enrollment data.
- `course_effectiveness_analysis.ipynb`: The Jupyter Notebook containing the Python code for data generation, cleaning, and clustering.
- `README.md`: This file, providing an overview of the project.

## Dataset Generation

A synthetic dataset of 20,000 course enrollments was generated using the `pandas` and `faker` libraries. The dataset includes the following columns:

- `course_id`: Unique identifier for each course.
- `student_id`: Unique identifier for each student.
- `enrollment_date`: Date of enrollment.
- `completion_status`: Binary indicator (0/1) of course completion, with a 25% dropout rate.
- `course_rating`: Rating of the course (1-5), skewed towards 3-4.
- `course_duration`: Duration of the course in weeks.
- `enrollment_fee`: Fee for the course.
- `student_satisfaction_score`: Student satisfaction score (1-5).
- `course_category`: Category of the course (e.g., STEM, Humanities).

Intentional data quality issues were introduced, including:

- Approximately 10% missing values across columns.
- Outliers in `course_duration`.
- Inconsistent category names (e.g., 'STEM' vs. 'ScienceTech').
- Seasonal enrollment patterns (e.g., fall spikes).

The generated dataset was saved as `course_effectiveness.csv`.

## Data Cleaning

The `course_effectiveness.csv` dataset was cleaned to address the introduced data quality issues using `pandas` and `scikit-learn`. The cleaning steps included:

- Handling missing values:
    - `course_rating` was imputed with the median.
    - `course_category` was imputed with the mode.
- Standardizing category names: 'ScienceTech' was replaced with 'STEM'.
- Removing outliers: Rows with `course_duration` greater than 52 weeks were removed.
- Encoding categorical variables: `course_category` was one-hot encoded.

The cleaned dataset was saved as `cleaned_course_effectiveness.csv` and loaded into an SQLite database named `course_enrollments.db`.

## K-means Clustering

K-means clustering was performed on the cleaned dataset to group courses based on their characteristics. The features used for clustering were:

- `course_rating`
- `course_duration`
- `enrollment_fee`

The steps involved were:

1.  **Feature Selection and Preprocessing**: The selected features were extracted and scaled using `StandardScaler`. Missing values in the selected features were imputed with the median.
2.  **Determining Optimal Clusters**: The elbow method was used to determine the optimal number of clusters by plotting the inertia for a range of cluster numbers (1 to 10). The plot suggested that 4 clusters was a reasonable choice.
3.  **Applying K-means**: The K-means algorithm was applied with 4 clusters, and the cluster labels were added to the cleaned DataFrame.
4.  **Evaluating Clustering**:
    - The silhouette score was calculated to evaluate the quality of the clustering. A silhouette score of approximately 0.278 was obtained.
    - A BI-focused completion rate KPI was calculated for each cluster by computing the mean of the `completion_status` for each cluster. The completion rates across the clusters were found to be similar (ranging from 0.744 to 0.754).
5.  **Visualizing Clusters**: Scatter plots of the feature pairs (`course_duration` vs. `enrollment_fee`, `course_rating` vs. `enrollment_fee`, and `course_rating` vs. `course_duration`) were generated, colored by cluster label, to visualize the cluster distribution.

## Summary and Next Steps

The analysis successfully generated a synthetic dataset with specified characteristics and data quality issues, cleaned the data, and performed K-means clustering.

**Key Findings:**

- The elbow method suggested 4 as the optimal number of clusters based on the selected features.
- The silhouette score indicated that the clusters are somewhat separated, but the separation is not very strong.
- The completion rates across the identified clusters were relatively similar, suggesting that the chosen features ('course\_rating', 'course\_duration', and 'enrollment\_fee') might not be the primary drivers of course completion in this synthetic dataset.

**Potential Next Steps:**

- **Explore other features**: Incorporate additional features (e.g., student demographics, engagement metrics, course category) into the clustering process to see if more distinct clusters with varying completion rates can be identified.
- **Characterize clusters**: Analyze the average values of the clustering features and other relevant columns for each cluster to understand the typical characteristics of courses within each group.
- **Apply different clustering algorithms**: Experiment with other clustering algorithms (e.g., DBSCAN, hierarchical clustering) to see if they yield different or improved results.
- **Predictive modeling**: Use the cleaned dataset to build predictive models for course completion or student satisfaction.
