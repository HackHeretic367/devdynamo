# devdynamo
dev
import json
import os

class Note:
    def __init__(self, title, content):
        self.title = title
        self.content = content

    def __str__(self):
        return f"Title: {self.title}\nContent: {self.content}"

class NoteManager:
    def __init__(self, filename="notes.json"):
        self.filename = filename
        self.notes = []
        self.load_notes()

    def load_notes(self):
        if os.path.exists(self.filename):
            with open(self.filename, "r") as file:
                notes_data = json.load(file)
                self.notes = [Note(**data) for data in notes_data]
        else:
            self.notes = []

    def save_notes(self):
        with open(self.filename, "w") as file:
            json.dump([note.__dict__ for note in self.notes], file, indent=4)

    def add_note(self, title, content):
        new_note = Note(title, content)
        self.notes.append(new_note)
        self.save_notes()

    def remove_note(self, title):
        self.notes = [note for note in self.notes if note.title != title]
        self.save_notes()

    def edit_note(self, title, new_title, new_content):
        for note in self.notes:
            if note.title == title:
                note.title = new_title
                note.content = new_content
                self.save_notes()
                break

    def display_notes(self):
        if not self.notes:
            print("No notes found.")
        else:
            for note in self.notes:
                print(note)
                print("-" * 20)

def main():
    manager = NoteManager()

    while True:
        print("\nNote Manager Menu")
        print("1. Add note")
        print("2. Remove note")
        print("3. Edit note")
        print("4. View notes")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            title = input("Enter note title: ")
            content = input("Enter note content: ")
            manager.add_note(title, content)
        elif choice == "2":
            title = input("Enter the title of the note to remove: ")
            manager.remove_note(title)
        elif choice == "3":
            title = input("Enter the title of the note to edit: ")
            new_title = input("Enter new note title: ")
            new_content = input("Enter new note content: ")
            manager.edit_note(title, new_title, new_content)
        elif choice == "4":
            manager.display_notes()
        elif choice == "5":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
