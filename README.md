# python-challenge - PyBank
import os
import csv
import locale

# Setting USD currency format
locale.setlocale(locale.LC_ALL, 'en_US.UTF-8')

# Path to collect data from the Resources folder
budget_csv = os.path.join("Resources", "budget_data.csv")

# Create empty list for Date & Profit/Losses
total_months = []
profit = []
total = 0
change = []

# Read CSV file with comma as the delimiter & headers
with open(budget_csv) as csv_file:
    csv_reader = csv.reader(csv_file, delimiter=",")
    
    # Reading the header row 
    csv_header = next(csv_reader)
    
    # Read the first row below the header
    first_row = next(csv_reader)
    previous_profit = int(first_row[1])
    total_months.append(first_row[0])
    profit.append(first_row[1])
    total = total + int(first_row[1])
               
       
    # Create for loop to go thru each row 
    for row in csv_reader:
        
        # Find the total months
        total_months.append(row[0])
                                   
        # Find total profits
        profit.append(row[1])
        total += int(row[1]) 
        
        # Find the change in profit 
        change.append(int(row[1])-previous_profit)
        previous_profit = int(row[1])
        
        # Find the greatest increase in profit with date
        greatest_increase = max(change)
        increase_date = total_months[change.index(greatest_increase)]            
        
        # Find the greatest decrease in profit 
        greatest_decrease = min(change)
        decrease_date = total_months[change.index(greatest_decrease)]
        
# Average change in Profit/Losses
average_change = round(sum(change)/len(change), 2)

#Print financial analysis summary
print(f"Financial Analysis")
print(f"____________________________")
print(f"Total Months:" , len(total_months))
print(f"Total:" , locale.currency(total))
print(f"Average Change:" , locale.currency(average_change))
print(f"Greatest Increase in Profits:" , (increase_date) , locale.currency(greatest_increase))
print(f"Greatest Decrease in Profits:" , (decrease_date) , locale.currency(greatest_decrease))

# Specify the file to write to
output_file = os.path.join("analysis", "financial_analysis.txt")

# Open the file using "write" mode.  
with open(output_file, 'w') as text:
    text.write(f"Financial Analysis\n")
    text.write(f"____________________________\n")
    text.write(f"Total Months: , len(total_months)\n")
    text.write(f"Total: , locale.currency(total)\n")
    text.write(f"Average Change: , locale.currency(average_change)\n")
    text.write(f"Greatest Increase in Profits: , (increase_date) , locale.currency(greatest_increase)\n")
    text.write(f"Greatest Decrease in Profits: , (decrease_date) , locale.currency(greatest_decrease)\n")
    
