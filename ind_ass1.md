# SQIT3073_A231
# Kee Loo Siang 287312

import sys
import os
os.system('cls' if os.name == 'nt' else 'clear')

# List to store the calculation
loan_calculations = []
# Defining function
DSR_threshold = 70
def calculate_monthly_installment(principal,annual_interest_rate,loan_term):
    number_of_payments = loan_term*12
    monthly_installment = (principal*(1+(annual_interest_rate/100)*loan_term))/number_of_payments
    return monthly_installment
def calculate_total_amount(monthly_installment,loan_term):
    number_of_payments = loan_term*12
    return monthly_installment*number_of_payments
def calculate_DSR(monthly_installment,monthly_commitments,monthly_income):
    return (monthly_installment+monthly_commitments)/monthly_income
def display_loan_information(principal,annual_interest_rate,loan_term,monthly_income,monthly_commitments):
    monthly_installment = calculate_monthly_installment(principal,annual_interest_rate,loan_term)
    total_amount = calculate_total_amount(monthly_installment,loan_term)
    dsr = calculate_DSR(monthly_installment,monthly_commitments,monthly_income)
    print ('\nLOAN INFORMATION:')
    print (f'Monthly Installment: RM{monthly_installment:.2f}')
    print (f'Total amount payable: RM{total_amount:.2f}')
    print (f'DSR threshold:{DSR_threshold:.2f}%')
    print (f'Debt Service Ratio (DSR):{dsr*100:.2f}%')
    if dsr < 0.70:
        print ('Congratulations! You are eligible for the loan application.')
    else:
        print ('Sorry, My dear. You are not eligible for the loan application due to high DSR.')
def display_new_loan_information(principal, annual_interest_rate, loan_term, monthly_income, monthly_commitments, new_dsr_threshold=None):
    monthly_installment = calculate_monthly_installment(principal,annual_interest_rate,loan_term)
    total_amount = calculate_total_amount(monthly_installment,loan_term)
    dsr = calculate_DSR(monthly_installment,monthly_commitments,monthly_income,)
    print ('\nLOAN INFORMATION:')
    print (f'Monthly Installment: RM{monthly_installment:.2f}')
    print (f'Total amount payable: RM{total_amount:.2f}')
    print (f'Debt Service Ratio (DSR):{dsr*100:.2f}%')
    print (f'DSR threshold:{DSR_threshold:.2f}%')
    if new_dsr_threshold is not None:
        print (f'New DSR threshold:{new_dsr_threshold:.2f}%')
        if dsr < new_dsr_threshold/100:
            print ('Congratulations! You are eligible for the loan application.')
        else:
            print ('Sorry, My dear. You are still not eligible for the loan application due to high DSR.')
    else:
        print (f'Debt Service Ratio (DSR):{dsr*100:.2f}%')
        if dsr < 0.70:
            print ('Congratulations! You are eligible for the loan application.')
        else:
            print ('Sorry, My dear. You are not eligible for the loan application due to high DSR.')
# Storing the calculation
    loan_calculation = {
        'Principal':f'RM{principal:.2f}',
        'Interest Rate':f'{annual_interest_rate:.2f}%',
        'Loan Term':f'{loan_term} years',
        'Monthly Income':f'RM{monthly_income:.2f}',
        'Monthly Commitments':f'RM{monthly_commitments:.2f}',
        'Monthly Installment':f'RM{monthly_installment:.2f}',
        'Total Amount Payable':f'RM{total_amount:.2f}',
        'DSR':f'{dsr*100:.2f}%',
        'Default DSR Threshold':f'{DSR_threshold}%',
    }
# Include new DSR threshold in the calculation if it's provided
    if new_dsr_threshold is not None:
        loan_calculation['New DSR Threshold']=f'{new_dsr_threshold:.2f}%'
# Append the calculation to the list
    loan_calculations.append(loan_calculation)
def display_all_loan_calculations():
    print ('\nALL LOAN CALCULATIONS:')
    for index,calculation in enumerate(loan_calculations, 1):
        print (f'\nCalculation {index}:')
        for key, value in calculation.items():
            print (f'{key}:{value}')
# Delete the previous calculation
def delete_calculation(index):
    try:
        deleted_calculation = loan_calculations.pop(index)
        print(f'Calculation {index +1 } deleted:')
        for key,value in deleted_calculation.items():
            print (f'{key}:{value}')
    except IndexError:
        print ('Invalid index. No calculation deleted')
while True:
    print ('\nMAIN MENU')
    print ('Press 1 to calculate a new loan.')
    print ('Press 2 to discover the loan calculations history.')
    print ('Press 3 to delete previous calculation')
    print ('Press 4 to exit the program.')
    try:
        selection = int(input('What would you like to do?:'))
# For the selection of 1, promt the user and check for the input validation
        if selection == 1:
            print ('\nCALCULATE A NEW LOAN')
            while True:
                try:
                    annual_interest_rate = float(input('Enter annual interest rate (%):'))
                    if (annual_interest_rate <= 0):
                        print ('Interest rate cannot be negative value, please re-enter.')
                    else: break
                except ValueError:
                    print ('Invalid input, please enter a valid number.')
            while True:
                try:
                    loan_term = int(input('Enter loan term in years:'))
                    if (loan_term <= 0):
                        print ('Loan term cannot be less than 1 years, please re-enter.')
                    else: break
                except ValueError:
                    print ('Invalid input, please enter a valid number.')
            while True:
                try:
                    monthly_income = float(input('Enter monthly income:'))
                    if (monthly_income <= 0):
                        print ('Monthly income cannot be negative value, please re-enter.')
                    else: break
                except ValueError:
                    print ('Invalid input, please enter a valid number.')
            while True:
                try:
                    monthly_commitments = float(input('Enter any other amount of monthly financial commitments, if no please enter 0: RM'))
                    if (monthly_commitments < 0):
                        print ('Amount of monthly financial commitments cannot be negative value, please re-enter.')
                    else: break
                except ValueError:
                    print ('Invalid input, please enter a valid number.')
            principal = float(input('Enter principal loan amount: RM'))
            if (principal >= (70*monthly_income-monthly_commitments)):
                print ('Exceeding maximum amount of loan based on your income, you are not eligible for the loan application.')
            else: 
                print ('The result is calculating...')
                display_loan_information(principal,annual_interest_rate,loan_term,monthly_income,monthly_commitments)
# Allow user select to modify the DSR threshold
                while True:
                    modify = str(input('Do you want to modify the DSR threshold? (Y/N):'))
                    if modify == 'Y':
                        new_dsr_threshold = float(input('Enter the new DSR threshold (%):'))
                        print ('The new result is re-generating...')
                        display_new_loan_information(principal, annual_interest_rate, loan_term, monthly_income, monthly_commitments, new_dsr_threshold)
                    elif modify == 'N':
                        display_new_loan_information(principal, annual_interest_rate, loan_term, monthly_income, monthly_commitments, new_dsr_threshold=None)
                        break
                    else:
                        print ('Invalid, please try again with a capital letter of Y or N ONLY.')
# For the selection 2, display all calculations
        elif selection == 2:
            display_all_loan_calculations()
# For the selection 3, allow user to delete previous calculation 
        elif selection == 3:
            display_all_loan_calculations()
            index_to_delete= int(input('Enter the index of the calculation to delete:'))
            delete_calculation (index_to_delete -1 )
# For the selection 4
        elif selection == 4:
            while True:
                k = str(input('Are you sure want to exit the system? (Y/N):')).upper()
                if k == 'Y':
                    print ('You are exiting the system. Goodbye my friend')
                    sys.exit()
                elif k == 'N':
                    print ('Thanks for staying with me, you are the best person I have ever met.')
                    break
                else:
                    print ('Invalid, please try again.')
        else:
            print ('Invalid, please enter a number between 1 and 4.')
    except ValueError:
        print ('Invalid input, please enter a valid number.')
