# Bank Data Anlysis Code


import os
import csv

#file to load and output

file_to_load = os.path.join(".","Resources","budget_data.csv")

file_to_output = os.path.join(".","budget_analysis.txt")

#print(file_to_load)
#print(file_to_output)


#initialising the variables

total_months = 0
total_amount_profitloss = 0
previous_net = 0
net_change_list = []
change_month=[]

#reading the csv file

with open (file_to_load) as csv_file:
    csv_reader = csv.reader(csv_file,delimiter=",")
    
    #print(csv_reader)
    
    #fining the header
    csv_header = next(csv_reader)
    
    #print(f"{csv_header} ------ HEADER")
    
    print()
    
    #looping to count the months
    for row in csv_reader:
        total_months += 1
        #adding the next row of profit/loss value 
        total_amount_profitloss += int(row[1])
        #calculating the change in profit/loss
        net_change = int(row[1])-previous_net
        #resetting the value of previous_net
        previous_net = int(row[1])
        
        #appending the profit/loss list with each value of change
        net_change_list.append(net_change)
        #appending the month change list with each month adding
        change_month.append(row[0])
        
    #print(change_month)
    #print(net_change_list)
        
    
    final_net_change_list = net_change_list[1:] # slicing the list since we do not need the first item of this list
    final_change_month_list = change_month[1:]  # slicing the list since we do need to match the previous list
    
average_net_change = sum(final_net_change_list)/len(final_net_change_list)    

#Combining the two lists as a dictionary
combine_change_month_dict = dict(zip(final_net_change_list,final_change_month_list))


#print(combine_change_month_dict)

greatest_increase_profit = max(final_net_change_list)
month_greatest_increase = combine_change_month_dict.get(greatest_increase_profit)





greatest_decrease_profit = min(final_net_change_list)  
month_greatest_decrease = combine_change_month_dict.get(greatest_decrease_profit)



    
        
with open (file_to_output,'w') as txt_file:
    budget_analysis=(
        f"      Financial Analysis\n"
        f"-------------------------------\n"
        f"Total months : {total_months}\n"      
        f"Total Profit/Loss : ${total_amount_profitloss}\n"    
        f"Average Change: ${average_net_change:.2f}\n"
        f"Greatest Increase in Profits: {month_greatest_increase} ({greatest_increase_profit})\n"
        f"Greatest Decrease in Profits: {month_greatest_decrease} ({greatest_decrease_profit})\n"
    )
    
    txt_file.write(budget_analysis)
    
    print(budget_analysis)
  
------------------------------------------------------------------------------------
------------------------------------------------------------------------------------

# Poll Data Analysis Code


import os
import csv

#joining the path and creating files to load and output

file_to_load = os.path.join(".","Resources","election_data.csv")
file_to_output = os.path.join(".","poll_analysis.txt")

# print(file_to_load)
# print(file_to_output)

#initializing the variables

totalvotes_number=0
win_count = 0
candidate_names = []
candidate_votes ={}


#reading the csv file
with open (file_to_load,"r") as csv_file:
    csv_reader = csv.reader(csv_file,delimiter=",")
    #print(csv_reader)

    
    #finding the header
    csv_header = next(csv_reader)
    #print(csv_header)
    print()
    
    #looping to count the total rows to calculate total vote
    for row in csv_reader:
        #print(row)
        
        totalvotes_number += 1

        candidate_name = row[2]
        
        #counting the nonrepeated candiadte names(rows) and adding it to the list
        if candidate_name not in candidate_names:     # new function "not in"
            
            #appending the blank list with candidate name if not repeating
            candidate_names.append(candidate_name)
            #resetting the next candidate's vote number to be zero
            candidate_votes[candidate_name]=0
            
          
        candidate_votes[candidate_name] += 1
            

    
    
    
    #print(candidate_names)
    #print(candidate_votes)
    
#writing a text file to print the results    
with open(file_to_output,"w") as txt_file:
    election_results = (
    f"Election Results\n"
    f"-----------------------\n"
    f"Total Votes {totalvotes_number}\n"
    f"-----------------------\n"
    
    )
    txt_file.write(election_results)
    print(election_results)
    
    
    # to calculate the vote percentage using loop to count each candidates vote
    for candidate in candidate_votes:
        votes = candidate_votes[candidate]
        vote_percentage = round(float(votes)/float(totalvotes_number)*100,2)
        #print(votes)
        #print(vote_percentage)
        
        #calculating the winner
        if (votes > win_count):
            win_count = votes
            winning_candidate = candidate
        
        voter_output =(f"{candidate}: {vote_percentage:.3f}% ({votes})\n")
        print(voter_output,end="")
        
      
        txt_file.write(voter_output)
    
    winning_summary=(
                    f"------------------------\n"
                    f"Winner: {winning_candidate}\n"
                    f"------------------------\n"
    )
    
    print(winning_summary)

    txt_file.write(winning_summary)


    


