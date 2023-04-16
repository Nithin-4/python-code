# python-code
import gspread
from oauth2client.service_account import ServiceAccountCredentials

# Set up authentication
scope = ['https://spreadsheets.google.com/feeds', 'https://www.googleapis.com/auth/drive']
creds = ServiceAccountCredentials.from_json_keyfile_name('credentials.json', scope)
client = gspread.authorize(creds)

# Open input sheets
input_sheet_1 = client.open('Python Assignment').worksheet('Input Sheet 1')
input_sheet_2 = client.open('Python Assignment').worksheet('Input Sheet 2')

# Read input data
input_data_1 = input_sheet_1.get_all_values()
input_data_2 = input_sheet_2.get_all_values()

# Open output sheets
output_sheet_1 = client.open('Python Assignment').worksheet('Output Sheet 1')
output_sheet_2 = client.open('Python Assignment').worksheet('Output Sheet 2')

# Clear previous output data
output_sheet_1.clear()
output_sheet_2.clear()

# Calculate output data for Output Sheet 1
output_data_1 = []
for row in input_data_1[1:]:
    total_score = sum([int(score) for score in row[1:]])
    output_data_1.append([row[0], str(total_score)])

# Write output data to Output Sheet 1
for i, row in enumerate(output_data_1):
    output_sheet_1.update_cell(i+2, 1, row[0])
    output_sheet_1.update_cell(i+2, 2, row[1])

# Calculate output data for Output Sheet 2
output_data_2 = []
for row in input_data_2[1:]:
    total_score = sum([int(score) for score in row[1:]])
    output_data_2.append([row[0], str(total_score)])

# Sort output data for Output Sheet 2
output_data_2.sort(key=lambda x: int(x[1]), reverse=True)

# Write output data to Output Sheet 2
for i, row in enumerate(output_data_2):
    output_sheet_2.update_cell(i+2, 1, row[0])
    output_sheet_2.update_cell(i+2, 2, row[1])
