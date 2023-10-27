import tkinter as tk
from tkinter import messagebox
from datetime import datetime

class Time:
    def init(self, hour, minute):
        self.hour = hour
        self.minute = minute

class BlablacarBooking:
    def init(self, driver_name, no_of_people, start_time, end_time, start_place, end_place):
        self.driver_name = driver_name
        self.no_of_people = no_of_people
        self.start_time = start_time
        self.end_time = end_time
        self.start_place = start_place
        self.end_place = end_place

    def is_valid(self):
        return (
            driver_name.isalpha() and
            1 <= no_of_people <= 4 and
            0 <= start_time.hour <= 23 and 0 <= start_time.minute <= 59 and
            0 <= end_time.hour <= 23 and 0 <= end_time.minute <= 59 and
            start_time.hour < end_time.hour and
            start_place.isalpha() and end_place.isalpha()
        )

class BlablacarCollection:
    def init(self):
        self.bookings = []

    def add_booking(self, booking):
        self.bookings.append(booking)

    def display_bookings(self):
        if self.bookings:
            for i, booking in enumerate(self.bookings, 1):
                print(f"#{i}: {booking.driver_name}, {booking.no_of_people}, "
                      f"{booking.start_time.strftime('%H:%M')} - {booking.end_time.strftime('%H:%M')}, "
                      f"{booking.start_place} to {booking.end_place}")
        else:
            messagebox.showinfo("Info", "No bookings to display")

class BlablacarApp:
    def init(self, root):
        self.root = root
        self.root.title("Blablacar Booking System")
        self.collection = BlablacarCollection()

        self.driver_name_entry = tk.Entry(root, width=30)
        self.no_of_people_entry = tk.Entry(root, width=30)
        self.start_time_entry = tk.Entry(root, width=30)
        self.end_time_entry = tk.Entry(root, width=30)
        self.start_place_entry = tk.Entry(root, width=30)
        self.end_place_entry = tk.Entry(root, width=30)

        self.driver_name_entry.grid(row=0, column=1)
        self.no_of_people_entry.grid(row=1, column=1)
        self.start_time_entry.grid(row=2, column=1)
        self.end_time_entry.grid(row=3, column=1)
        self.start_place_entry.grid(row=4, column=1)
        self.end_place_entry.grid(row=5, column=1)

        self.driver_name_label = tk.Label(root, text="Driver Name")
        self.no_of_people_label = tk.Label(root, text="No. of People")
        self.start_time_label = tk.Label(root, text="Start Time (HH:MM)")
        self.end_time_label = tk.Label(root, text="End Time (HH:MM)")
        self.start_place_label = tk.Label(root, text="Start Place")
        self.end_place_label = tk.Label(root, text="End Place")

        self.driver_name_label.grid(row=0, column=0)
        self.no_of_people_label.grid(row=1, column=0)
        self.start_time_label.grid(row=2, column=0)
        self.end_time_label.grid(row=3, column=0)
        self.start_place_label.grid(row=4, column=0)
        self.end_place_label.grid(row=5, column=0)

        self.add_booking_button = tk.Button(root, text="Add Booking", command=self.add_booking)
        self.display_bookings_button = tk.Button(root, text="Display Bookings", command=self.display_bookings)

        self.add_booking_button.grid(row=6, column=0, columnspan=2)
        self.display_bookings_button.grid(row=7, column=0, columnspan=2)

    def add_booking(self):
        driver_name = self.driver_name_entry.get()
        no_of_people = self.no_of_people_entry.get()
        start_time_str = self.start_time_entry.get()
        end_time_str = self.end_time_entry.get()
        start_place = self.start_place_entry.get()
        end_place = self.end_place_entry.get()

        try:
            start_time = datetime.strptime(start_time_str, '%H:%M')
            end_time = datetime.strptime(end_time_str, '%H:%M')
        except ValueError:
            messagebox.showerror("Error", "Invalid time format (HH:MM)")
            return
new_booking = BlablacarBooking(driver_name, no_of_people, start_time, end_time, start_place, end_place)

        if new_booking.is_valid():
            self.collection.add_booking(new_booking)
            messagebox.showinfo("Success", "Booking added successfully")
        else:
            messagebox.showerror("Error", "Invalid booking data")

    def display_bookings(self):
        self.collection.display_bookings()

if name == "main":
    root = tk.Tk()
    app = BlablacarApp(root)
    root.mainloop()
