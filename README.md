Apps-Cleaning-and-Organizing-data

Unit 1 Guided Project: Python for Data Scientist

Data sets: Applestore.csv and Googleplaystore.csv 

In this guided project we use everything learned in the "python for data scientist" course to clean a data set. Google and Apple app datas sets wereused for this project and i willel eaborate as i go on through this program


open_file = open('AppleStore.csv')
from csv import reader
read_file = reader(open_file)
ios = list(read_file)
ios_header = ios[0]
ios = ios[1:]


open_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(open_file)
android = list(read_file)
android_header = android[0]
android = android[1:]

print(ios_header)

print(android_header)


# I print both my headers just to check that my code is working properly with no issues


# In the following funcation was made pickout a data slice from a data set and  prints each row rowof the dataset with a space after each row we added an true or false statement to print the header of rows and columns in a data set if there is a header if there is one (if true)

def explore_data(dataset, start, end, rows_and_columns=False):
    dataset_slice = dataset[start:end]    
    for row in dataset_slice:
        print(row)
        print('\n') # adds a new (empty) line after each row

    if rows_and_columns:
        print('Number of rows:', len(dataset))
        print('Number of columns:', len(dataset[0]))
        print('\n')

# We used our exploredata() fucntion to retrieve the headers and a small slice of our google and apple data sets

print(ios_header)
print('\n')     
explore_data(ios, 0, 3, True)

print(android_header)
print('\n')
explore_data(android,0, 3, True)

# we were asked to find a row that didnt belong and one of the hints that we got was to check to see if one of our rows was missing a category from one of the columns

# I used a for loop to help me find the row that was missing a category by counting the number of elements in each row and printing whatever row had less elements

dup = []
for rows in android:
    element_count = len(android[0])
    if len(rows) != element_count:
        print(rows)
        
        
# knowing what that row contains then led me to run another for loop to finding the index of this element in the data set and then deleting it. I created a list to number off each row, from there i just added eavery row from the android list to it. I then used this for loop after to find the row that conatined the app name in android[0], which in turn returned a number that i had added in my list numm[]. I was able to find my index this way to know what row to delete that had missing data. 

numm=[]
num = 0
for rows in android:
    num = num + 1
    result = str(rows) + str(num)
    numm.append(result)

for rows in numm:
    if "Life Made" in rows: 
        print(rows)

# In the following two for loops we are able to filter out duplicates to know if there are certain rows in our android data set that we have to get rid of. By creating a seperate dataset, we are able to loop through the dataset and place all duplicates in this new data set which we can know use to delete the duplicates.

for app in android:
    name = app[0]
    if name == 'Instagram':
        print(app)
        
# We can find some fo the dpulicates here. We printed 15 rows of app names to know what apps we are looking at, and to verify that there are some 
# Another thing that was done with the following piece of code is making sure that we can keepp only one of the apps in the data set by also filtering out the highest amount of reviews from the duplicates and using a dictionary to keep only the apps that are in the data set once
                     
        
duplicate_apps = []
unique_apps = []

for app in android:
    name = app[0]
    if name in unique_apps:
        duplicate_apps.append(name)
    else:
        unique_apps.append(name)
print('Number of duplicate apps', len(duplicate_apps))
print('\n')
print('Examples of duplicate apps:', duplicate_apps[:15])


reviews_max = {}

for app in android:
    name = app[0]
    n_reviews = float(app[3])
    
    if name in reviews_max and reviews_max[name] < n_reviews:
        reviews_max[name] = n_reviews
        
    elif name not in reviews_max:
        reviews_max[name] = n_reviews
        
print('Expected length:', len(android) - 1181)
print('Actual length:', len(reviews_max))

# Here is where we took out duplicate apps with the same amount of reviews, making sure to focuse on only having one app with one name

android_clean = []
already_added = []

for app in android:
    name = app[0]
    n_reviews = float(app[3])
    
if (reviews_max[name] == n_reviews) and (name not in already_added):
        android_clean.append(app)
        already_added.append(name)


# All we are doing here is looking at some rows using our explore function

explore_data(android_clean, 0, 3, True)


# the following two pieces of code extracted the english apps from both the apple and google apps. we used this function to then show you a couple of rows of what was extracted to make sure no other characters were present. 

def english_characters(string):
    non_ascii = 0
    
    for eng_chr in string:
        if  ord(eng_chr) > 127:
            non_ascii += 1
           
    if non_ascii > 3:
        return False
    else:
        return True


eng_android =[]
eng_ios = []

for app in android_clean:
    name = app[0]
    if english_characters(name):
        eng_android.append(app)
        
for app in ios:
    name = app[1]
    if english_characters(name):
        eng_ios.append(app)
        
explore_data(eng_android, 0, 3, True)
print('\n')
explore_data(eng_ios, 0, 3, True)

# This last step is to clear out the free apps from the non-free apps

android_final = []
ios_final = []

for app in eng_android:
    price = app[7]
    if price == '0':
        android_final.append(app)
        
for app in eng_ios:
    price = app[4]
    if price == '0.0':
        ios_final.append(app)
        
print(len(android_final))
print(len(ios_final))

# we have finished sorting out, english apps, free apps, inaccurate data, and duplicate data from the data sets, but to make sure that we get the most from the information we have we are going to also look at genres and the success rate measured by numbers in reviews and downloads on both platforms. This is to ensure that everyone gets the most out of the the app overall

def freq_table(data_set, index):
    frequency_table = {}
    total = 0
    
    for element in data_set:
        total += 1
        genre = element[index]
        if genre in frequency_table:
            frequency_table[genre] += 1
        else:
            frequency_table[genre] = 1
            
    table_percentages = {}
    for key in frequency_table:
        percentage = (frequency_table[key]/total) * 100
        table_percentages[key] = percentage
        
    return table_percentages

def display_table(dataset, index):
    table = freq_table(dataset, index)
    table_display = []
    for key in table:
        key_val_as_tuple = (table[key], key)
        table_display.append(key_val_as_tuple)

    table_sorted = sorted(table_display, reverse = True)
    for entry in table_sorted:
        print(entry[1], ':', entry[0])
        
genres_ios = freq_table(ios_final, -5)

for genre in genres_ios:
    total = 0
    len_genre = 0
    for app in ios_final:
        genre_app = app[-5]
        if genre_app == genre:            
            n_ratings = float(app[5])
            total += n_ratings
            len_genre += 1
    avg_n_ratings = total / len_genre
    print(genre, ':', avg_n_ratings)
    
    After going
