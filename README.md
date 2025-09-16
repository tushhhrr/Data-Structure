# Data-Structure

SENTINEL = -999

class WeatherRecord:
    def __init__(self, year: int, city: int, temp: float):
        self.year = year
        self.city = city
        self.temp = temp

    def __repr__(self):
        return f"WeatherRecord(year={self.year}, city={self.city}, temp={self.temp})"


class WeatherDataStorage:
    def __init__(self, years: int, cities: int):
        self.years = years
        self.cities = cities
        self.data = [[SENTINEL for _ in range(cities)] for _ in range(years)]

    def insert(self, year_idx: int, city_idx: int, temp: float):
        self.data[year_idx][city_idx] = temp

    def delete(self, year_idx: int, city_idx: int):
        self.data[year_idx][city_idx] = SENTINEL

    def retrieve(self, year_idx: int, city_idx: int):
        val = self.data[year_idx][city_idx]
        return None if val == SENTINEL else val

    def row_major_access(self):
        for yi in range(self.years):
            for ci in range(self.cities):
                yield yi, ci, self.data[yi][ci]

    def column_major_access(self):
        for ci in range(self.cities):
            for yi in range(self.years):
                yield yi, ci, self.data[yi][ci]

    def pretty_print(self):
        header = [f"City{i}" for i in range(self.cities)]
        print("Year\City | " + " | ".join(header))
        print("-" * (10 + 6 * self.cities))
        for yi in range(self.years):
            row = []
            for ci in range(self.cities):
                val = self.data[yi][ci]
                row.append("-" if val == SENTINEL else str(val))
            print(f"{yi:8} | " + " | ".join(row))


if __name__ == "__main__":
    storage = WeatherDataStorage(years=3, cities=4)
    storage.insert(0, 0, 25.3)
    storage.insert(0, 2, 27.8)
    storage.insert(1, 1, 30.2)
    storage.insert(2, 3, 22.0)

    print("Retrieved:", storage.retrieve(1, 1))

    storage.delete(0, 2)

    storage.pretty_print()

    print("\nRow-major traversal:")
    for yi, ci, v in storage.row_major_access():
        print(yi, ci, v)

    print("\nColumn-major traversal:")
    for yi, ci, v in storage.column_major_access():
        print(yi, ci, v)
