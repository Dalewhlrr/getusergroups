import getpass
import subprocess

def get_selection_from_user(options, prompt):
    response = 0
    valid_response = False

    while not valid_response:
        option_no = 0

        print(prompt)
        print("[0]: Cancel")

        for option in options:
            option_no += 1
            print("[{}]: {}".format(option_no, option))

        try:
            response = int(input())
        except ValueError:
            continue

        if response == 0:
            return ''
        elif response <= option_no:
            valid_response = True

    return options[response - 1]

domain = "#"

# Prompt for domain admin credentials
username = input("Enter Your Domain Admin Credentials username: ")
username_with_domain = f"{username}@{domain}"
password = getpass.getpass("Enter your password: ")

# Loop until valid AD account is found
valid = False
while not valid:
    username_input = input("Enter Employee Username (samAccountName): ")

    # Find user
    try:
        powershell_command = [
            "powershell",
            "-Command",
            f"Get-AdUser -Identity {username_input} -Properties MemberOf | Select-Object -ExpandProperty MemberOf"
        ]

        result = subprocess.run(powershell_command, capture_output=True, text=True, check=True)
        output = result.stdout.strip().split('\n')

        file_path = "usergroups.txt"

        with open(file_path, 'w') as file:
            file.write('\n'.join(output))

        print(output)
        valid = True
    except subprocess.CalledProcessError:
        print("User Doesn't Exist")
