# Data-Cleaning
# Task 1: Find out the missing values in each column
col_missing_values = df.isnull().sum()
print("Missing values in each column:\n", col_missing_values)

# Task 2: Drop the column 'reviews_per_month' permanently
df.drop(columns='reviews_per_month', inplace=True)

# Task 3: Drop the rows having more than 1 missing value
df_rows_dropped = df.dropna(thresh=len(df.columns) - 1)

# Task 4: Fill the 21 missing values in 'host_name' with 'Airbnb'
host_total = df['host_name'].fillna('Airbnb', inplace=False)

# Task 5: Check if any name in the 'host_name' column has digits
has_digits = df['host_name'].str.contains('\d').sum()
if has_digits > 0:
    print(f"The column has {has_digits} rows with digits.")
else:
    print("No digits found in the column.")

# Task 6: Fill missing values in 'price' with the mean
mean_price = df['price'].mean()
mean_df_price = df['price'].fillna(mean_price, inplace=False)

# Task 7: Fill missing values in 'last_review' using forward fill
ffill_review = df['last_review'].fillna(method='ffill')

# Task 8: Select duplicate hosts based on 'name', 'host_id', and 'price'
duplicate_hosts = df[df.duplicated(subset=['name', 'host_id', 'price'], keep=False)]

# Task 9: Drop duplicates (answer to multiple choice is a) First and d) Last)
# Dropping duplicates and keeping the first non-NaN value
df_unique_hosts = df.drop_duplicates(subset=['name', 'host_id', 'price'], keep='first')

# Task 10: Count all Private rooms in 'room_type'
private_rooms_counts = df['room_type'].value_counts().get('Private room', 0)

# Task 11: Find names that contain the substring 'park'
names_having_park = df['name'][df['name'].str.contains('park', case=False, na=False)]

# Task 12: Replace 'Kitchen' with 'Restaurant' in 'neighbourhood'
kitchen_to_restaurant = df['neighbourhood'].replace('Kitchen', 'Restaurant')

# Task 13: Split 'room_type' column to find room/home/apt
roomOrhome = df['room_type'].str.split().str[1]

# Task 14: Remove rows with invalid values in 'availability_365' (0 values)
df_invalid_availability = df[df['availability_365'] != 0]

# Task 15: Choose the correct option for filling NaN values
# Most common: d) Mean

# Task 16: Identify outliers in 'minimum_nights' column (43 std or more)
mean_min_nights = df['minimum_nights'].mean()
std_min_nights = df['minimum_nights'].std()
df_nights = df.copy()
df_nights['Min_Nights_cleaned'] = df['minimum_nights'].apply(
    lambda x: x if abs(x - mean_min_nights) < 43 * std_min_nights else None)

# Task 17: Identify outliers in 'price' column using IQR
Q1 = df['price'].quantile(0.25)
Q3 = df['price'].quantile(0.75)
IQR = Q3 - Q1
df_Price = df.copy()
df_Price['Price_cleaned'] = df['price'].apply(
    lambda x: x if (x >= Q1 - 1.5 * IQR) and (x <= Q3 + 1.5 * IQR) else None)
