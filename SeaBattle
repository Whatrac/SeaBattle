def get_coord(prompt='Введите координату (например, A1): '):
    while True:
        coord = input(prompt)
        if len(coord) >= 2:
            x, y = coord[0].upper(), coord[1:]
            if x in 'ABCDEFGHIJ' and y.isdigit() and 1 <= int(y) <= 10:
                x = 'ABCDEFGHIJ'.index(x)
                y = int(y) - 1
                return x, y
        print('Некорректный ввод. Попробуйте снова.')
class Ship:
    def __init__(self, length, orientation, x, y):
        self.length = length
        self.orientation = orientation
        self.x = x
        self.y = y
        self.hits = 0
    def is_sunk(self):
        return self.hits >= self.length
class Game:
    def __init__(self):
        self.field = [['.'] * 10 for _ in range(10)]
        self.ships = []
    def can_place_ship(self, ship):
        for i in range(ship.length):
            if ship.orientation == 'H':
                if ship.x + i >= 10 or self.field[ship.y][ship.x + i] != '.':
                    return False
            else:
                if ship.y + i >= 10 or self.field[ship.y + i][ship.x] != '.':
                    return False
        return True
    def place_ship(self, ship):
        if self.can_place_ship(ship):
            for i in range(ship.length):
                if ship.orientation == 'H':
                    self.field[ship.y][ship.x + i] = '@'
                else:
                    self.field[ship.y + i][ship.x] = '@'
            self.ships.append(ship)
        else:
            print("Невозможно разместить корабль.")
    def shoot(self, x, y):
        if self.field[y][x] == '@':
            self.field[y][x] = 'X'
            for ship in self.ships:
                if ship.orientation == 'H' and ship.y == y and ship.x <= x < ship.x + ship.length:
                    ship.hits += 1
                elif ship.orientation == 'V' and ship.x == x and ship.y <= y < ship.y + ship.length:
                    ship.hits += 1
            print("Попадание!")
            return True
        elif self.field[y][x] == '.':
            self.field[y][x] = '*'
            print("Промах!")
            return False
        else:
            print("Эта координата уже была обстреляна.")
            return False
    def all_ships_sunk(self):
        return all(ship.is_sunk() for ship in self.ships)
    def show_field(self):
        print("  A B C D E F G H I J")
        for i, row in enumerate(self.field):
            print(f"{i + 1} {' '.join(row)}")
def main():
    print("Игрок 1 размещает корабли")
    p1 = Game()
    for _ in range(2):  # Игрок 1 размещает 2 корабля
        length = int(input("Введите длину корабля (1-4): "))
        orientation = input("Введите ориентацию (H - горизонтально, V - вертикально): ").upper()
        x, y = get_coord("Введите начальные координаты для размещения корабля: ")
        ship = Ship(length, orientation, x, y)
        p1.place_ship(ship)
    print("Игрок 2 размещает корабли")
    p2 = Game()
    for _ in range(2):  # Игрок 2 размещает 2 корабля
        length = int(input("Введите длину корабля (1-4): "))
        orientation = input("Введите ориентацию (H - горизонтально, V - вертикально): ").upper()
        x, y = get_coord("Введите начальные координаты для размещения корабля: ")
        ship = Ship(length, orientation, x, y)
        p2.place_ship(ship)
    # Игровой цикл
    while True:
        for player, opponent in [(p1, p2), (p2, p1)]:
            print(f"\nХод Игрока {'1' if player == p1 else '2'}:")
            player.show_field()  # Показываем только поле текущего игрока
            print("Поле противника скрыто.")
            while True:
                x, y = get_coord("Введите координаты для атаки: ")
                if opponent.shoot(x, y):  # Игрок стреляет по полю противника
                    if opponent.all_ships_sunk():
                        print(f"Игрок {'1' if opponent == p1 else '2'} победил!")
                        return
                    print("У вас еще один ход!")  # Дополнительный ход после попадания
                else:
                    break  # Неудачный выстрел, выходим из внутреннего цикла
if __name__ == '__main__':
    main()
