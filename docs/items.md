#Items

Item information contains type, possible sub-type and item id. Item ID is uint64 number and belogs to same ID-space with bots and other map elements. In order to pick an item the bot must stand on top that particular item. When item is dropped it will drop to same place where the bot is standing. Items can be dropped only to empty terrain spaces ".".

##Item types, sub-types and type specific fields
- key
- weapon
  - damage
  - type
    - melee
    - gun
      - ammo
- bomb
  - timer
