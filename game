
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_NAME 30

// global linked list 'list' contains the list of maps
struct mapList {
	struct map* map;
	struct mapList* next;
} *list = NULL;					// currently empty list



// structure "map" contains the map's name, enemy count, hero pointer, and linked list of enemies
struct map {
	char name[MAX_NAME];
	unsigned int enemyCount;
	struct hero* hero;
	struct enemy* enemyList;		// linked list 'enemies' contains a list of enemies on the map
};

//  structure 'enemy' contains the enemy's stats 
struct enemy {
	char name[MAX_NAME];
	int hp;
	int attack;
	struct enemy* next;
};

// structure 'hero' contains the hero's stats
struct hero {
	char name[MAX_NAME];
	int hp;
	int attack;
	char location[MAX_NAME];
} *theHero = NULL;

// forward declaration of functions (already implmented)
void flushStdIn();
void executeAction(char);
void displayHero();
void displayMapList(struct mapList* tempList);
void displayMapEnemyList(struct mapList* tempList);

void addMap(char* mapNameInput, unsigned int enemyCount); 						
struct map* searchMap(char* mapNameInput);	      							
void createHero(char* heroNameInput, unsigned int heroHpInput, unsigned int heroAttackInput); 		
void moveHero(char* mapNameInput); 									
void addEnemy(char* mapNameInput, char* enemyNameInput, unsigned int enemyHpInput, unsigned int enemyAttackInput);	
void removeMap(char* mapNameInput);											
int mapCombat(char* mapNameInput);												
int combatRecurse(struct hero* theHero, struct enemy* enemyList);		


int main()
{
	char selection = 'a';		// initialized to a dummy value
	do
	{
		printf("\nCSE240 HW 5\n");
		printf("\t a: add a new map to the list\n");
		printf("\t d: display map list (no enemies)\n");
		printf("\t b: search for a map on the list\n");
		printf("\t h: create or redo the hero\n");
		printf("\t m: move the hero to a new map\n");
		printf("\t i: display hero information\n");
		printf("\t c: add an enemy to a map\n");
		printf("\t l: display maps that have a specific enemy\n");
		printf("\t r: remove a map\n");
		printf("\t f: conduct combat on a map\n");
		printf("\t q: quit\n");
		printf("Please enter your selection:\n");

		selection = getchar();
		flushStdIn();
		executeAction(selection);
	} while (selection != 'q');

	return 0;
}

// flush out leftover '\n' characters
void flushStdIn()
{
	char c;
	do c = getchar();
	while (c != '\n' && c != EOF);
}

// Ask for details from user for the given selection and perform that action
// Read the function case by case
void executeAction(char c)
{
	char mapNameInput[MAX_NAME], enemyNameInput[MAX_NAME], heroNameInput[MAX_NAME];
	unsigned int heroHpInput, heroAttackInput, enemyHpInput, enemyAttackInput;
	int combatOutcome = 0;
	struct mapList* tempList = NULL;

	switch (c)
	{
	case 'a':						// add map
								// input map details from user
		printf("\nPlease enter map's name: ");
		fgets(mapNameInput, sizeof(mapNameInput), stdin);
		mapNameInput[strlen(mapNameInput) - 1] = '\0';	// discard the trailing '\n' char

		if (searchMap(mapNameInput) == NULL)					
		{
			addMap(mapNameInput, 0); //Pass static zero into the function as all maps will start with zero enemies
			printf("\nThe map was successfully added to the list!\n");
		}
		else
			printf("\nThat map is already on the list!\n");
		break;

	case 'd':						// display the map list
		tempList = list;
		displayMapList(list);
		break;

	case 'b':						// search for a map on the list
		printf("\nPlease enter a map's name: ");
		fgets(mapNameInput, sizeof(mapNameInput), stdin);
		mapNameInput[strlen(mapNameInput) - 1] = '\0';	// discard the trailing '\n' char

		if (searchMap(mapNameInput) == NULL)				
			printf("\nThe map does not exist or the list is empty! \n\n");
		else
		{
			printf("\nThe map exists on the list! \n\n");
		}
		break;
	
	case 'h':
		printf("\nPlease enter the hero's name: ");
		fgets(heroNameInput, sizeof(heroNameInput), stdin);
		heroNameInput[strlen(heroNameInput) - 1] = '\0';	// discard the trailing '\n' char
		printf("\nPlease enter the hero's Hp Stat: ");
		scanf("%d", &heroHpInput);	
		flushStdIn();
		printf("\nPlease enter the hero's Attack Stat: ");
		scanf("%d", &heroAttackInput);
		flushStdIn();
		createHero(heroNameInput, heroHpInput, heroAttackInput);
		printf("\nThe Hero was created! \n\n");
		break;

	case 'm':
		printf("\nPlease enter the map's name: ");
		fgets(mapNameInput, sizeof(mapNameInput), stdin);
		mapNameInput[strlen(mapNameInput) - 1] = '\0';	// discard the trailing '\n' char

		if (searchMap(mapNameInput) == NULL)					
			printf("\nThe map does not exist or the list is empty! \n\n");
		else{
			moveHero(mapNameInput);
			printf("\nThe Hero has been moved! \n\n");
		}
		break;

	case 'r':						// remove a map
		printf("\nPlease enter a map's name: ");
		fgets(mapNameInput, sizeof(mapNameInput), stdin);
		mapNameInput[strlen(mapNameInput) - 1] = '\0';	// discard the trailing '\n' char

		if (searchMap(mapNameInput) == NULL)					
			printf("\nThe map does not exist or the list is empty! \n\n");
		else
		{
			removeMap(mapNameInput);
			printf("\nThe map was successfully removed from the list! \n\n");
		}
		break;
	
	case 'i':
		displayHero();
		break;

	case 'c':						// add an enemy
		printf("\nPlease enter the map's name: ");
		fgets(mapNameInput, sizeof(mapNameInput), stdin);
		mapNameInput[strlen(mapNameInput) - 1] = '\0';	// discard the trailing '\n' char

		if (searchMap(mapNameInput) == NULL)					
			printf("\nThe map does not exist or the list is empty! \n\n");
		else
		{
			printf("\nPlease enter the enemy's name: ");
			fgets(enemyNameInput, sizeof(enemyNameInput), stdin);
			enemyNameInput[strlen(enemyNameInput) - 1] = '\0';	// discard the trailing '\n' char
			printf("\nPlease enter the enemy's Hp Stat: ");
			scanf("%d", &enemyHpInput);
			flushStdIn();
			printf("\nPlease enter the enemy's Attack Stat: ");
			scanf("%d", &enemyAttackInput);
			flushStdIn();
			addEnemy(mapNameInput, enemyNameInput, enemyHpInput, enemyAttackInput);
			printf("\nThe enemy was added! \n\n");
		}
		break;

	case 'l':					// list the maps an enemy shows up in
		tempList = list;
		displayMapEnemyList(tempList);
		break;
	
	case 'f':
		printf("\nPlease enter the map's name: ");
		fgets(mapNameInput, sizeof(mapNameInput), stdin);
		mapNameInput[strlen(mapNameInput) - 1] = '\0';	// dscanf("%d", &IDNumInput);iscard the trailing '\n' char

		if (searchMap(mapNameInput) == NULL)					
			printf("\nThe map does not exist or the list is empty! \n\n");
		else
		{
			combatOutcome = mapCombat(mapNameInput);
			if(combatOutcome == 0)
				printf("\nThe hero was slain! \n\n");
			else if(combatOutcome == 1)
				printf("\nThe hero was victorious! \n\n");
			else if(combatOutcome == 2)
				printf("\nThe hero is not currently in this map. \n \n");
			else
				printf("\nThis map has no enemies for the hero to fight! \n\n");
		}
		break;

	case 'q':					// quit
		break;

	default: printf("%c is invalid input!\n", c);
	}
}

// Displays the Global hero
void displayHero(){
	if(theHero == NULL)
		printf("\nThe Hero does not exist. \n");
	else{
		printf("\nThe Hero is here!");
		printf("\nThe Hero's name: %s", theHero->name);
		printf("\nThe Hero's current hp: %d", theHero->hp);
		printf("\nThe Hero's attack stat: %d", theHero->attack);
		printf("\nThe Hero's current location: %s\n", theHero->location);
	}
}

// Displays the maps in the mapList
void displayMapList(struct mapList* tempList)
{
	// write the code below.
	if (tempList == NULL)			// check if end of list is reached
	{
		printf("\n (end of list) \n");
		return;
	}
	else
	{
		printf("\nMap name: %s", tempList->map->name);
		printf("\nEnemy Count: %d", tempList->map->enemyCount);
		if(tempList->map->hero == NULL)
			printf("\nThere is no hero here. \n");
		else{
			printf("\n\nThere is a hero here.");
			printf("\nHero name: %s", tempList->map->hero->name);
			printf("\nHero current hp: %d", tempList->map->hero->hp);
			printf("\nHero attack stat: %d\n", tempList->map->hero->attack);
		}

		displayMapList(tempList->next);	// move on to next map
	}
}

// This function searches for all the maps that have a specific type of enemy present
// For example, if the enemy name was "goblin", it would search for all maps that have instances of
// "goblin" in their enemy list. It then prints the 
void displayMapEnemyList(struct mapList* tempList)
{
	// write the code below.scanf("%d", &IDNumInput);
	if (tempList == NULL)
	{
		printf("\nList is empty!\n");      	// check if list is empty
		return;
	}
	char enemyNameInput[MAX_NAME];
	printf("\nPlease enter enemy's name: ");
	fgets(enemyNameInput, sizeof(enemyNameInput), stdin);
	enemyNameInput[strlen(enemyNameInput) - 1] = '\0';
	int enemyPresent = 0;
	while (tempList != NULL) 			// traverse the list to display 'map' elements
	{
		struct enemy* enList = tempList->map->enemyList;
		if (enList != NULL)			// check if enemies are present in the map
		{
			int found = 0; 			// integer acting as a bool to check if the enemy was found in each map
			while (enList != NULL && found == 0)          // traverse the list of enemies to display
			{
				if (strcmp(enList->name, enemyNameInput) == 0)
				{
					printf("\nMap name: %s", tempList->map->name);
					printf("\nEnemy Count: %d", tempList->map->enemyCount);
					if(tempList->map->hero == NULL)
						printf("\nThere is no hero here. \n");
					else{
						printf("\n\nThere is a hero here.");
						printf("\nHero name: %s", tempList->map->hero->name);
						printf("\nHero current hp: %d", tempList->map->hero->hp);
						printf("\nHero attack stat: %d\n", tempList->map->hero->attack);
					}
					enemyPresent = 1;
					found = 1;
					break;
				}
				enList = enList->next;
			}
		}
		tempList = tempList->next;               // move on to the next map
	}
	if (enemyPresent == 0)
	{
		printf("\n There are no maps with the enemy %s present. \n", enemyNameInput);
	}
}


void addMap(char* mapNameInput, unsigned int enemyCount)
{
	// Create a new 'map' node and initialize its properties.
	struct map* tempMap = (struct map*)malloc(sizeof(struct map));
	strcpy(tempMap->name, mapNameInput);
	tempMap->enemyCount = enemyCount;
	tempMap->hero = NULL;
	tempMap->enemyList = NULL;
	
	// Create a new 'mapList' node, to give a values of above map.
	struct mapList* tempMapNode = (struct mapList*)malloc(sizeof(struct mapList));
	tempMapNode->map = tempMap;
	tempMapNode->next = list;
	list = tempMapNode;
	
}

struct map* searchMap(char* mapNameInput)
{
	struct mapList* tempList = list;
	
	for(;tempList != NULL;){
	    if(strcmp(mapNameInput, tempList->map->name) == 0){ //comparing the two names
	        return tempList->map;
	    }
	    tempList = tempList->next;
	}
	return NULL;
}


void createHero(char* heroNameInput, unsigned int heroHpInput, unsigned int heroAttackInput){
	// Write your code below.
	if(theHero != NULL){
	    struct mapList* tempList = list;	
	    // Locate and remove the hero from their current map.
    	for(;tempList != NULL;){
    	    if(tempList->map->hero == theHero){
    	        tempList->map->hero = NULL;
    	    }
    	    tempList = tempList->next;
    	}
	}
	// Create a new hero, initialize their properties, and set the global 'theHero'.
	theHero = (struct hero*)malloc(sizeof(struct hero));
	theHero->attack = (int)heroAttackInput;
	theHero->hp = (int)heroHpInput;
	strcpy(theHero->name, heroNameInput);
	strcpy(theHero->location, "Nowhere");
	
}

void moveHero(char* mapNameInput){
	struct mapList* tempList = list;		// Create tempList to iterate through the list.
	for(;tempList != NULL;){
	    if(strcmp(tempList->map->name, mapNameInput) == 0){
	        if(tempList->map->hero != theHero){
	            // If the hero is not in the current map, find their current map and remove them.
	            struct mapList* tempList2 = list;
	            for(;tempList2 != NULL;){
            	    if(tempList2->map->hero == theHero){
            	        tempList2->map->hero = NULL;
            	        break;
            	    }
            	    tempList2 = tempList2->next;
            	}
            	// Set theHero in the new map and update the location.
            	tempList->map->hero = theHero;
            	strcpy(theHero->location, mapNameInput);
	        }
	    }
	    tempList = tempList->next;
	}
}

void addEnemy(char* mapNameInput, char* enemyNameInput, unsigned int enemyHpInput, unsigned int enemyAttackInput)
{
	struct mapList* tempList = list;	
	for(;tempList != NULL;){
	    if(strcmp(tempList->map->name, mapNameInput) == 0){ // comparing the names
	        struct enemy* newEnemy = (struct enemy*)malloc(sizeof(struct enemy));
	        newEnemy->next = NULL;
	        newEnemy->attack = enemyAttackInput;
	        newEnemy->hp = enemyHpInput;
	        strcpy(newEnemy->name, enemyNameInput);
	        // If the 'enemies' list is empty, add the enemy; otherwise, add it to the end.
	        if (tempList->map->enemyList == NULL) {
                tempList->map->enemyList = newEnemy;
            } else {
                struct enemy* currentEnemy = tempList->map->enemyList;
                while (currentEnemy->next != NULL) {
                    currentEnemy = currentEnemy->next;
                }
                currentEnemy->next = newEnemy;
            }
            // Update the enemy count for the map.
            tempList->map->enemyCount++;
	    }
	    tempList = tempList->next;
	}
}

void removeMap(char* mapNameInput)
{
	struct mapList* tempList = list;
	struct mapList* prev = NULL;			
	struct enemy* tempEnemy;
	for(;tempList != NULL;){
	    if(strcmp(tempList->map->name, mapNameInput) == 0){
	        // If the hero is in the map, remove it from the global scope.
	        if(tempList->map->hero == theHero){
	            theHero = NULL;
	        }
	        // Remove the map's enemy list and its global reference.
	        tempList->map->enemyList = NULL;
	        // If the map is at the head of the list, update the list.
	        if(prev == NULL){
	            list = tempList->next;
	        }
	        else{
	            prev->next = tempList->next;
	        }
	        break;
	    }
	    prev = tempList;
	    tempList = tempList->next;
	}
}

int mapCombat(char* mapNameInput){
	struct mapList* tempList = list;			
	for(;tempList != NULL;){
	    if(strcmp(tempList->map->name, mapNameInput) == 0){// Once the map is located, check if the hero is in the map.
	         if (tempList->map->hero == NULL) {
                return 2;// Return 2 if the hero is not in the map.
            }
            if (tempList->map->enemyCount == 0) {
                return 3;// Return 3 if there are no enemies on the map.
            }
            // If both the hero and enemies are present, enter the combatRecurse() function.
            int result = combatRecurse(tempList->map->hero, tempList->map->enemyList);
            if(result == 0){
              // If the hero dies, free theHero, update the map hero to NULL, and set the global theHero to NULL.
              theHero = NULL;
              tempList->map->hero = NULL;
            }
            else{
              // If the hero survives, reset the enemyList to NULL.
              tempList->map->enemyList = NULL;
            }
            return result;
	    }
	    tempList = tempList->next;
	}
	return 0;
}

int combatRecurse(struct hero* Hero, struct enemy* enemyList){
	if (Hero->hp <= 0) {
        return 0;// Return 0 if the hero's health reaches or goes below 0.
    }
    if (enemyList == NULL) {
        return 1;// Return 1 if there are no enemies left, indicating the hero wins.
    }
    // a round of combat by adjusting health points based on attacks.
    Hero->hp -= enemyList->attack;
    enemyList->hp -= Hero->attack;

    if (enemyList->hp <= 0) {
        enemyList = enemyList->next;
    }
    // going to the next round of combat.
    return combatRecurse(Hero, enemyList);
}
