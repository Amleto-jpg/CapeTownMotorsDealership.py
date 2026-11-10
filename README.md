 class Car:
    def __init__(self, name, model, year, price, mileage):
        self.name = name
        self.model = model
        self.year = year
        self.price = price
        self.mileage = mileage
        self.is_sold = False

    def describe(self):
        return f"{self.year} {self.name} {self.model} R{self.price:,.0f} - {self.mileage:,} km"

    def age(self, current_year):
        return current_year - self.year

    def drive(self, km):
        if km < 0:
            print("Error: distance cannot be negative")
        else:
            self.mileage += km

    def apply_discount(self, percent):
        if 1 <= percent <= 50:
            self.price *= (1 - percent / 100)
        else:
            print("Error: discount must be between 1 and 50")

    def sell(self):
        if self.is_sold:
            print("This car has already been sold")
        else:
            self.is_sold = True
            print(f"{self.name} {self.model} sold!") 


class ElectricCar(Car):
    def __init__(self, name, model, year, price, mileage, battery_range):
        super().__init__(name, model, year, price, mileage)
        self.battery_range = battery_range

    def describe(self):
        return f"{self.year} {self.name} {self.model} R{self.price:,.0f} - {self.mileage:,} km - Range: {self.battery_range} km"                    


class Dealership:
    def __init__(self, name):
        self.name = name
        self.cars = []

    def add_car(self, car):
        self.cars.append(car)

    def show_all(self):
        for i, car in enumerate(self.cars, start=1):
            print(f"{i}. {car.describe()}")

    def available_cars(self):
        return [car for car in self.cars if not car.is_sold]

    def find_by_name(self, name):
        return [car for car in self.cars if car.name.lower() == name.lower()]

    def newer_than(self, year):
        return [car for car in self.cars if car.year > year]

    def total_stock_value(self):
        return sum(car.price for car in self.available_cars())  

    def sell_car(self, model):
        for car in self.available_cars(): 
            if car.model.lower() == model.lower():
                car.sell()
                return car
        print("No available car with that model")
        return None


# --- MAIN PROGRAM ---
if __name__ == "__main__":
    # Task 1: Create a dealership called "Cape Town Motors"
    print("--- Task 1 ---")
    dealership = Dealership("Cape Town Motors")
    print(f"Dealership created: {dealership.name}")

    # Task 2: Create these 6 cars and add them to the dealership
    print("\n--- Task 2 ---")
    car1 = Car("Mazda", "CX 5", 2026, 620000, 0)
    car2 = Car("Toyota", "Hilux", 2024, 650000, 15000)
    car3 = Car("Honda", "Y30", 2003, 45000, 210000)
    car4 = Car("Land Rover", "F6", 2019, 480000, 90000)
    car5 = Car("Toyota", "Corolla", 2021, 310000, 40000)
    car6 = Car("VW", "Polo", 2018, 175000, 85000)

    for c in [car1, car2, car3, car4, car5, car6]:
        dealership.add_car(c)
    print("6 cars added to the dealership.")

    # Task 3: Show all cars
    print("\n--- Task 3 ---")
    dealership.show_all()

    # Task 4: Print the age of every car (current year = 2026)
    print("\n--- Task 4 ---")
    current_year = 2026
    for car in dealership.cars:
        print(f"{car.name} {car.model}: {car.age(current_year)} years old")

    # Task 5: Drive the Polo 500 km, then try -100 km. Print its new mileage.
    print("\n--- Task 5 ---")
    car6.drive(500)
    car6.drive(-100)
    print(f"Polo mileage: {car6.mileage:,} km")

    # Task 6: Give the Honda a 20% discount, then try 70%. Print its new price.
    print("\n--- Task 6 ---")
    car3.apply_discount(20)
    car3.apply_discount(70)
    print(f"Honda price: R{car3.price:,.0f}")

    # Task 7: Find and print all Toyotas
    print("\n--- Task 7 ---")
    toyotas = dealership.find_by_name("Toyota")
    for car in toyotas:
        print(car.describe())

    # Task 8: Print all cars newer than 2020
    print("\n--- Task 8 ---")
    newer_cars = dealership.newer_than(2020)
    for car in newer_cars:
        print(car.describe()) 

    # Task 9: Sell the Hilux, then try to sell it again
    print("\n--- Task 9 ---")
    dealership.sell_car("Hilux")
    dealership.sell_car("Hilux")

    # Task 10: Try to sell a model that does not exist, e.g. "Civic"
    print("\n--- Task 10 ---")
    dealership.sell_car("Civic")

    # Task 11: Print how many cars are still available and total stock value
    print("\n--- Task 11 ---")
    print(f"Available cars: {len(dealership.available_cars())}")
    print(f"Total stock value: R{dealership.total_stock_value():,.0f}")

    # --- BONUS ELECTRIC CAR ---
    print("\n--- Bonus: Electric Car ---")
    electric_car = ElectricCar("BYD", "Dolphin", 2025, 450000, 0, 400)
    dealership.add_car(electric_car)
    dealership.show_all()
    print(f"Available cars (including EV): {len(dealership.available_cars())}")
    print(f"Total stock value (including EV): R{dealership.total_stock_value():,.0f}")
