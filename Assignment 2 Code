import sys

class ArtworkLocation:
    PERMANENT_GALLERIES = "Permanent Galleries"
    EXHIBITION_HALLS = "Exhibition Halls"
    OUTDOOR_SPACES = "Outdoor Spaces"

class Artwork:
    def __init__(self, title, artist, date_of_creation, historical_significance, location):
        self.title = title
        self.artist = artist
        self.date_of_creation = date_of_creation
        self.historical_significance = historical_significance
        self.location = location

class Exhibition:
    def __init__(self, name, start_date, end_date, location):
        self.name = name
        self.start_date = start_date
        self.end_date = end_date
        self.location = location
        self.artworks = []
    
    def add_artwork(self, artwork):
        self.artworks.append(artwork)

class Visitor:
    def __init__(self, name, age, ID, visitor_type, student_teacher_id=None):
        self.name = name
        self.age = age
        self.ID = ID
        self.visitor_type = visitor_type
        self.student_teacher_id = student_teacher_id
        self.tickets = []
        self.is_in_group = False
        self.group = None

class Group:
    def __init__(self):
        self.visitors = []
        self.size = 0

    def add_visitor(self, visitor):
        self.visitors.append(visitor)
        self.size += 1
        visitor.is_in_group = True
        visitor.group = self  # Link back to the group for discount calculation

class Ticket:
    BASE_PRICE = 63  # Base price for adults in AED
    VAT_RATE = 0.05  # 5% VAT

    def __init__(self, visitor, event):
        self.visitor = visitor
        self.event = event
        self.price = self.calculate_price()

    def calculate_price(self):
        if self.visitor.age < 18 or self.visitor.age > 60 or self.visitor.visitor_type in ['teacher', 'student']:
            return 0
        else:
            price_before_vat = Ticket.BASE_PRICE
            if self.visitor.is_in_group and self.visitor.group.size >= 15:
                price_before_vat *= 0.5
            return round(price_before_vat * (1 + Ticket.VAT_RATE), 2)

class Museum:
    def __init__(self):
        self.exhibitions = []
        self.visitors = []

    def add_exhibition(self, exhibition):
        self.exhibitions.append(exhibition)
    
    def add_visitor(self, visitor):
        self.visitors.append(visitor)

museum = Museum()

def add_new_art():
    title = input("Enter artwork title: ")
    artist = input("Enter artist name: ")
    date_of_creation = input("Enter date of creation: ")
    historical_significance = input("Enter historical significance: ")
    location = input("Enter location (1- Permanent Galleries, 2- Exhibition Halls, 3- Outdoor Spaces): ")
    location_dict = {"1": ArtworkLocation.PERMANENT_GALLERIES, "2": ArtworkLocation.EXHIBITION_HALLS, "3": ArtworkLocation.OUTDOOR_SPACES}
    exhibition_location = location_dict.get(location, ArtworkLocation.PERMANENT_GALLERIES)
    
    artwork = Artwork(title, artist, date_of_creation, historical_significance, exhibition_location)
    exhibition_name = input("Enter exhibition name: ")
    start_date = input("Enter exhibition start date: ")
    end_date = input("Enter exhibition end date: ")
    exhibition = Exhibition(exhibition_name, start_date, end_date, exhibition_location)
    exhibition.add_artwork(artwork)
    museum.add_exhibition(exhibition)
    print("Artwork and exhibition added successfully.")

def get_valid_number_of_visitors(prompt, min_val=1, max_val=40):
    while True:
        try:
            value = int(input(prompt))
            if min_val <= value <= max_val:
                return value
            else:
                print(f"Error: Please enter a number between {min_val} and {max_val}.")
        except ValueError:
            print("Invalid input, please enter a numerical value.")

def book_ticket():
    number_of_visitors = get_valid_number_of_visitors("How many visitors are booking tickets? ", 1, 40)
    group = Group() if 15 <= number_of_visitors <= 40 else None

    for _ in range(number_of_visitors):
        name = input("Enter visitor name: ")
        age = int(input("Enter visitor age: "))
        visitor_id = input("Enter visitor ID: ")
        visitor_type = "adult" if 18 <= age <= 60 else "child" if age < 18 else "senior"
        is_teacher_or_student = input("Is the visitor a teacher or student? (yes/no): ").lower() == "yes"
        student_teacher_id = input("Enter teacher/student ID: ") if is_teacher_or_student else None
        visitor = Visitor(name, age, visitor_id, visitor_type, student_teacher_id)
        if group:
            group.add_visitor(visitor)
        ticket = Ticket(visitor, "Exhibition/Event Name")
        visitor.tickets.append(ticket)
        museum.add_visitor(visitor)
    
    print(f"{number_of_visitors} ticket(s) booked successfully.")

def display_all_information():
    print("Museum: Louvre, Location: Abu Dhabi, UAE")
    for exhibition in museum.exhibitions:
        print(f"Exhibition: {exhibition.name}, Location: {exhibition.location}")
        for artwork in exhibition.artworks:
            print(f"  Artwork: {artwork.title}, Artist: {artwork.artist}")
    total_price = 0
    for visitor in museum.visitors:
        print(f"Visitor: {visitor.name}, Age: {visitor.age}, ID: {visitor.ID}, Type: {visitor.visitor_type}, Student/Teacher ID: {visitor.student_teacher_id if visitor.student_teacher_id else 'N/A'}")
        for ticket in visitor.tickets:
            print(f"  Event: {ticket.event}, Price: {ticket.price} AED")
            total_price += ticket.price
    print(f"Total Price for all tickets: {total_price} AED")

def main_menu():
    while True:
        print("\nMain Menu:")
        print("1- Add new art to the museum")
        print("2- Book a ticket for a visitor(s)")
        print("3- Display all information")
        print("4- End the process")
        
        choice = input("Please enter your choice: ")
        
        if choice == '1':
            add_new_art()
        elif choice == '2':
            book_ticket()
        elif choice == '3':
            display_all_information()
        elif choice == '4':
            print("Ending the process. Thank you for using the system.")
            break
        else:
            print("Invalid choice, please try again.")

if __name__ == "__main__":
    main_menu()
