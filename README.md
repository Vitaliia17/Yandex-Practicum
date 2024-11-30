# Yandex-Practicum
practice for 2nd Sprint

import datetime as dt
from decimal import Decimal

DATE_FORMAT = '%Y-%m-%d'
goods = {}

def add(items, title, amount, expiration_date=None):
    if title not in items:
        items[title] = []
    if expiration_date:
        expiration_date = dt.datetime.strptime(expiration_date, DATE_FORMAT).date()
    items[title].append({
        'amount': amount,
        'expiration_date': expiration_date
    })

# Пример использования    
add(goods, 'Яйца', Decimal('10'), '2023-09-30')
print(goods)

add(goods, 'Яйца', Decimal('3'), '2023-10-15')
print(goods)

add(goods, 'Вода', Decimal('3.5'))
print(goods)


def add_by_note(items, note):
    parts = note.split(' ')
    print(parts) 
    if len(parts) > 2 and '-' in parts[-1]:  
        expiration_date = parts[-1]
        good_amount = Decimal(parts[-2])
        title = ' '.join(parts[:-2])
    else:
        expiration_date = None
        good_amount = Decimal(parts[-1])
        title = ' '.join(parts[:-1])
  
    add(items, title, good_amount, expiration_date)

# Пример использования
add_by_note(goods, 'Яйца 10 2023-09-30')
print(goods)


def find(items, needle):
    result = []
    for title in items:
        if needle.lower() in title.lower():
            result.append(title)
    return result

def amount(items, needle):
    total_amount = Decimal('0')
    for title, batches in items.items():
        if needle.lower() in title.lower():
            for batch in batches: 
                total_amount += batch['amount']
    return total_amount


def expire(items, in_advance_days=0):
    today = dt.date.today()
    expired_goods = [] 
    for title, batches in items.items():
        total_expired_amount = Decimal('0')
        for batch in batches:
            expiration_date = batch['expiration_date']
            if expiration_date and expiration_date <= today +dt.timedelta(days = in_advance_days):
                total_expired_amount += batch['amount']
        if total_expired_amount > 0:
            expired_goods.append((title, total_expired_amount))
    return expired_goods
