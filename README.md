# Apps-Cleaning-and-Organizing-data

Unit 1 Guided Project: Python for Data Scientist

# Data sets: Applestore.csv and Googleplaystore.csv
# In this guided project we use everything learned in the 
# "python for data scientist" course to clean a data set.
# Google and Apple app datas sets wereused for this project and i
# willel eaborate as i go on through this program


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
# I print both my headers justto check that my code is working properly with no issues


# In the following funcation was made pickout a data slice from a data set 
# and  prints each row rowof the dataset with a space after each row
# we added an true or false statement to print the header of rows and columns 
# in a data set if there is a header
# if there is one (if true)

def explore_data(dataset, start, end, rows_and_columns=False):
    dataset_slice = dataset[start:end]    
    for row in dataset_slice:
        print(row)
        print('\n') # adds a new (empty) line after each row

    if rows_and_columns:
        print('Number of rows:', len(dataset))
        print('Number of columns:', len(dataset[0]))
        print('\n')

# We used our exploredata() fucntion to retrieve the headers and a small
# slice of our google and apple data sets

print(ios_header)
print('\n')     
explore_data(ios, 0, 3, True)

print(android_header)
print('\n')
explore_data(android,0, 3, True)

# we were asked to find a row that didnt belong and one of the hints 
# that we got was to check to see if one of our rows was missing a category 
# from one of the columns
# I used a for loop to help me find the row that was missing a category by 
# counting the number of elements in each row and printing whatever row
# had less elements

dup = []
for rows in android:
    element_count = len(android[0])
    if len(rows) != element_count:
        print(rows)
        
        
# knowing what that row contains then led me to run another for loop to finding the index of this element in the data set and then deleting it

numm=[]
num = 0
for rows in android:
    num = num + 1
    result = str(rows) + str(num)
    numm.append(result)

for rows in numm:
    if "Life Made" in rows: 
        print(rows)
