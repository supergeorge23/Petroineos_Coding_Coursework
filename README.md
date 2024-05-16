# Project Summary
This project centers around the SymbolsUpdate class, which manages financial symbol data through a series of CSV files using a Python-based Jupyter notebook, answer.ipynb. The class comprises three primary functions:

`load_new_data_from_file`: Loads and processes data from symbols_update_n.csv files.\
`save_new_data`: Appends processed data to database.csv.\
`get_data_from_database`: Retrieves the most recent updates for each symbol from database.csv.\

## Function Details
### `load_new_data_from_file` : 
#### Purpose:
Loads and processes data from specified CSV files (symbols_update_n.csv) into a structured DataFrame, ready for further processing and analysis.

#### Description:
This function performs several key tasks:
Reads CSV Files: Attempts to load data from a CSV file into a pandas DataFrame. If unsuccessful, it logs an error and returns an empty DataFrame with appropriate columns.
#### Processes Data:
Adds a current timestamp to track data processing time.
Maps country codes to full names, defaulting to NaN if codes are missing or unrecognized.
Ensures data integrity by handling missing or corrupt data fields (symbol, hold, item, item_value) by replacing them with NaN.
Generates New Entries: Transforms each row into two rows based on 'isin' and 'cusip' identifiers, capturing both along with all associated data.
#### Parameters:
file_path (str): Path to the CSV file to be processed.
#### Returns:
processed_data (DataFrame): Contains processed data with columns ["symbol", "hold", "country", "item", "item_value", "updatedby", "updatetime"]. Ensures each processed entry is timestamped and tagged with the data handler's identity.
#### Corner Cases:
File Access Errors: If the CSV file cannot be read due to missing file or permissions issues, the function handles this gracefully by returning an empty DataFrame.
####Unmapped Country Codes: If a country code is not in the predefined map or is improperly formatted, the country field is set to NaN.
Data Integrity Issues: Replaces any missing or corrupted required data fields with NaN to maintain the integrity of the dataset.


### `save_new_data` : 
#### Purpose:
Saves loaded data into the database.csv file, effectively appending new records or creating the file if it doesn't exist.
#### Description:
This function ensures data persistence by appending new DataFrame entries to an existing database file. Key operations include:
1.  File Existence Check: 
Before attempting to write data, it checks whether the file exists and is not empty to determine if headers should be included.
2.  Data Appending: 
The function appends new data to database.csv, with appropriate error handling to manage file access and writing issues.
#### Parameters:
input_data (pd.DataFrame): The DataFrame containing data to be saved to the database.
#### Error Handling:
1. File Access Errors: Handles errors related to file existence checks. If an error occurs, it logs the issue and halts further execution to prevent data corruption.
2. Write Failures: If writing to the file fails (due to permissions, disk space, etc.), the error is logged, and the function ceases operation without altering the file.

#### Output:
On successful execution, new records are appended to database.csv. If errors occur, they are logged to the console, and no changes are made to the database.



### `get_data_from_database` : 
#### Purpose:
Retrieves the most recently updated data for every symbol in database.csv, ensuring that only the latest entries are used for each unique symbol and item combination.
#### Description:
This function efficiently manages data retrieval from a structured database, performing several key tasks:
- Read Database: Loads all data from database.csv into a DataFrame.
- Data Sorting and Filtering:
1. Converts the updatetime column to datetime format for accurate sorting.
2. Sorts entries based on updatetime in descending order to position the most recent entries first.
3. Drops duplicates for each combination of symbol, item, and item_value, keeping only the most recent entry.
4. Re-sorts data alphabetically by the symbol column to facilitate easier lookup and analysis.
- Reset: Resets the DataFrame index after sorting to maintain a clean and sequential index.
#### Parameters:
None. The function operates directly on the database.csv file.

#### Returns:
latest_data (DataFrame): A DataFrame containing the latest entries for each symbol and item, with columns organized as ["symbol", "hold", "country", "item", "item_value", "updatedby", "updatetime"].
#### Error Handling:
File Not Found: If database.csv does not exist, the function returns an empty DataFrame with the appropriate columns. This ensures that the calling code can handle the result uniformly without additional error checking.

### `display_database_contents` : 
#### Purpose:
Displays the contents of the database.csv file in a human-readable format, optimized for console or terminal viewing. This function is optional and primarily used for debugging, verification, or presentation purposes to review the current state of the database without needing to manually open the file.

#### Description:
This function enhances the readability of data by adjusting the display settings of pandas DataFrames, ensuring that all data columns and their full contents are visible when printed. Key operations include:

#### Adjust Display Settings: Configures pandas display options to ensure that the DataFrame is displayed without truncation:
- display.max_columns: Shows all columns in the DataFrame, regardless of their number.
- display.width: Utilizes the maximum available width for display, adapting to the console's or terminal's width.
- display.max_colwidth: Ensures that the content of each column is fully displayed, irrespective of the content's length.
Load and Display Data: Reads the entire database.csv file into a DataFrame and prints it to the console, allowing for immediate visual inspection of its contents.


