class Car:
    def __init__(self, comfort_class: int, clean_mark: int, brand: str) -> None:
        self.comfort_class = comfort_class
        self.clean_mark = clean_mark
        self.brand = brand


class CarWashStation:
    def __init__(self, distance_from_city_center: float, clean_power: int, 
                 average_rating: float, count_of_ratings: int) -> None:
        self.distance_from_city_center = distance_from_city_center
        self.clean_power = clean_power
        self.average_rating = average_rating
        self.count_of_ratings = count_of_ratings

    def serve_cars(self, cars: list[Car]) -> float:
        total_income = 0
        for car in cars:
            if car.clean_mark < self.clean_power:
                total_income += self.calculate_washing_price(car)
                car.clean_mark = self.clean_power
        return round(total_income, 1)

    def calculate_washing_price(self, car: Car) -> float:
        price = (car.comfort_class * (self.clean_power - car.clean_mark) *
                 self.average_rating) / self.distance_from_city_center
        return round(price, 1)

    def wash_single_car(self, car: Car) -> None:
        if car.clean_mark < self.clean_power:
            car.clean_mark = self.clean_power

    def rate_service(self, rating: int) -> None:
        self.count_of_ratings += 1
        self.average_rating = round(((self.average_rating * (self.count_of_ratings - 1)) + rating) / self.count_of_ratings, 1)


# Example Usage:

# Create Car objects
bmw = Car(comfort_class=3, clean_mark=3, brand="BMW")
audi = Car(comfort_class=4, clean_mark=9, brand="Audi")

print(bmw.clean_mark)  # 3

# Create a CarWashStation object
wash_station = CarWashStation(
    distance_from_city_center=5,
    clean_power=6,
    average_rating=3.5,
    count_of_ratings=6
)

# Serve cars and calculate income
income = wash_station.serve_cars([bmw, audi])
print(income)  # 6.3
print(bmw.clean_mark)  # 6

# Another example where both cars get washed
bmw = Car(comfort_class=3, clean_mark=3, brand="BMW")
audi = Car(comfort_class=4, clean_mark=2, brand="Audi")

print(bmw.clean_mark)  # 3
print(audi.clean_mark)  # 2

income = wash_station.serve_cars([bmw, audi])
print(income)  # 17.5

print(bmw.clean_mark)  # 6
print(audi.clean_mark)  # 6

# Calculate washing price for a car
ford = Car(comfort_class=2, clean_mark=1, brand="Ford")
wash_cost = wash_station.calculate_washing_price(ford)
print(wash_cost)  # 9.1

# Rate the service
wash_station.rate_service(5)

print(wash_station.average_rating)  # 4.0
print(wash_station.count_of_ratings)  # 12
