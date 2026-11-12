# digital_diary

from datetime import datetime

FILE_NAME = "diary.txt"

#read all notes from the file
def read_notes():
    notes = []

    try:
        with open(FILE_NAME, "r") as file:
            for line in file:
                line = line.strip()

                if "|" in line:
                    date, text = line.split("|", 1)
                    notes.append((date, text))
    except FileNotFoundError:
        pass
    return notes

#Add a new note
def add_note():
    date = input("Enter Date(yyyy-mm-dd): ")

    try:
        valid_date = datetime.strptime(date, "%Y-%m-%d")

        if valid_date.strftime("%Y-%m-%d") != date:
            print("Invalid date, use yyyy-mm-dd.")
            return

    except ValueError:
        print("Invalid date, Use yyyy-mm-dd.")
        return

    text = input("Enter your note text: ").strip()

    if text == "":
        print("No text can be empty.")
        return

    with open(FILE_NAME, "a") as file:
        file.write(date + "|" + text + "\n")

    print("NOTE SAVED.")

#View all notes
def view_notes():
    notes = read_notes()

    if len(notes) == 0:
        print("No notes found. Your diary is empty.")
        return

    print("\n===== ALL DIARY NOTES =====")

    for i, note in enumerate(notes, start=1):
        date, text = note
        print(f"{i} | {date} | {text}")

#Search notes using a keyword
def search_notes():
    notes = read_notes()

    if len(notes) == 0:
        print("No notes found. Your diary is empty.")
        return

    keyword = input("Enter a keyword to search: ").strip().lower()

    if keyword == "":
        print("Search keyword cannot be empty.")
        return

    found = False

    print("\n===== SEARCH RESULTS =====")

    for i, note in  enumerate(notes, start=1):
        date, text = note

        if keyword in text.lower():
            print(f"{i} | {date} | {text}")
            found = True

    if not found:
        print("No matching notes found.")

# Delete a note using its number

def delete_note():
    notes = read_notes()

    if len(notes) == 0:
        print("No notes found. Your diary is empty.")
        return

    print("\n===== ALL DIARY NOTES =====")

    for i, note in enumerate(notes, start=1):
        date, text = note
        print(f"{i} | {date} | {text}")

    try:
        number = int(input("Enter note number to delete: "))

    except ValueError:
        print("Invalid note number.")
        return

    if number < 1 or number > len(notes):
        print(f"Note number {number} not found.")
        return

    notes.pop(number - 1)

    with open(FILE_NAME, "w") as file:
        for note in notes:
            date, text = note
            file.write(date + "|" + text + "\n")

    print(f"Note {number} deleted.")
    print(f"Notes left: {len(notes)}")


# Main menu

def main():
    while True:
        print("\n===== DIGITAL DIARY =====")
        print("1. Add Note")
        print("2. View All Notes")
        print("3. Search Notes")
        print("4. Delete Note")
        print("5. Exit")

        choice = input("Enter menu option: ")

        if choice == "1":
            add_note()

        elif choice == "2":
            view_notes()

        elif choice == "3":
            search_notes()

        elif choice == "4":
            delete_note()

        elif choice == "5":
            print("Thank you for using Digital Diary!")

            break

        else:
            print("Invalid menu option. Please try again.")

#Start the program 
if __name__ == "__main__":
    main()
