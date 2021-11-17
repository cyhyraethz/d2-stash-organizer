### Current plan

1. Update the game data files with patch 1.00 txt files.

   Some modifications may be necessary here since ItemStatCost.txt is written in a very different format,
   some text files are missing (ItemTypes.txt, Properties.txt, Runes.txt, Sets.txt, SkillDesc.txt), and
   there's only TreasureClass.txt instead of TreasureClassEx.txt.

   Thinking of replacing the missing files with empty files, renaming TreasureClass -> TreasureClassEx, and
   rewriting src/game-data/parsing/itemStats.ts to work with the original format of the ItemStatCost.txt file.

2. Reverse engineer the bit offsets for the various item properties
   (e.g. sockets, inventory position, quality, level, id, etc).

3. Modify the parsing scripts to work with the old save file formats.

### How to reverse engineer offsets

1. Write a script to write an item from a save file into its own item file (26 bytes, starting with JM).

2. Find items that are exactly the same except for a single property (e.g. item level, durability, sockets, etc).

3. Transfer items to compare onto a mule with no other items (not even starting weapons, potions, or scrolls).

4. Run the script to write the items onto their own individual save files for comparison.

5. Compare the items in a text editor bit by bit (e.g. with xxd -b for binary digits instead of hexdump).

6. Try to find the bit, or string of bits, that is responsible for the single property that is different.

7. Test it by editing the item in a character save file and modifying those bits to effect the property.

8. Take notes if able to change only that one specific property (e.g. modifying the number of sockets).

### Potentially useful strategies

Start with the easier properties to compare (durability, sockets, etc), and with the properties that could 
make finding items to test easier if those bits can be ruled out and ignored (once their purpose is known)
such as item level.
