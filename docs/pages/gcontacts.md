## Importable csv

Batch convert plain csv file with mobile numbers to google contacts importable csv. This also removes duplicates.

``` py
import csv

numlist = []

with open('math.csv', mode='r') as file:
    csv_reader = csv.DictReader(file)

    for row in csv_reader:
        numlist.append(row['first'])

filtred = set(numlist)

formatted = {}
for x, num in enumerate(filtred):
    item = x % 256
    base = x // 256

    formatted[f'CM{base:02d}-{item:03d}'] = num


with open('new_math.csv', mode='w') as file:

    fieldnames = [
        'Name',
        'Given Name',
        'Additional Name',
        'Family Name',
        'Yomi Name',
        'Given Name Yomi',
        'Additional Name Yomi',
        'Family Name Yomi',
        'Name Prefix',
        'Name Suffix',
        'Initials',
        'Nickname',
        'Short Name',
        'Maiden Name',
        'Birthday',
        'Gender',
        'Location',
        'Billing Information',
        'Directory Server',
        'Mileage',
        'Occupation',
        'Hobby',
        'Sensitivity',
        'Priority',
        'Subject',
        'Notes',
        'Language',
        'Photo',
        'Group Membership',
        'Phone 1 - Type',
        'Phone 1 - Value'
    ]


    writer = csv.DictWriter(file, fieldnames=fieldnames)
    writer.writeheader()

    for x, y in formatted.items():
        writer.writerow({'Phone 1 - Value': y, 'Name': x})
```