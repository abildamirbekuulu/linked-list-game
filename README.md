C program that manages a linked list of maps for a video game. The program allows you to perform various actions, including adding maps, searching for maps, creating a hero, moving the hero to different maps, adding enemies to maps, displaying information about the hero and maps, removing maps, and conducting combat on a map.

Here's a brief explanation of the important functions:

addMap: Adds a new map to the linked list of maps. It creates a new map node and inserts it at the beginning of the list.

searchMap: Searches for a map in the list by name. It returns the map node if found, otherwise NULL.

createHero: Creates a hero character and assigns it to the global variable theHero. If a hero already exists, it removes the hero from their current map and recreates the hero.

moveHero: Moves the hero to a new location on a map. It also updates the hero's location.

addEnemy: Adds an enemy to a specified map's list of enemies. If the map does not exist, or if the hero is not in the map, it does nothing.

removeMap: Removes a map from the list. It also removes the hero from that map if present and deletes the map's enemies.

mapCombat: Initiates combat on a map. It checks if the hero is in the map, if enemies are present, and then enters the recursive combatRecurse function for combat simulation. It returns outcomes such as 0 for hero death, 1 for enemy victory, 2 for the hero not being in the map, and 3 for no enemies on the map.

combatRecurse: Recursively simulates combat between the hero and enemies. It handles rounds of combat by adjusting health points based on attacks and returns outcomes such as 0 for hero death, 1 for enemy victory, and continues until one side wins.

Overall, the code manages maps, heroes, and enemies in a game world and allows for various interactions with these entities. It ensures proper memory management and data integrity throughout the game simulation.
