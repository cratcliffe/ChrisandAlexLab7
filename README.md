# ChrisandAlexLab7
import java.util.Scanner;

public class Player {

	String playerName;
	Scanner scan = new Scanner(System.in);
	
	public Player(){
		Hand hand = new Hand();
		playerName = "";
	}
	
	public Player(String name){
		Hand hand = new Hand();
		playerName = name;
	}
	
	public boolean takeTurn(){
		String card;
		displayHand();
		
		if(Hand.isEmpty())
			pickUp();
		if(Hand.isFull())
			drop();
		
		pickUpOrDrop();
		
		return completeTurn();
	}
	
	public String getName(){
		return playerName;
	}
	
	public void pickUpOrDrop(){
		int input;
		boolean done = true;
		
		while(done)
		System.out.print("Please enter whether to pick up or drop off(enter 1 for pick up or enter 2 for drop):");
		input = scan.nextInt();
		
		if(input == 1){
			pickUp();
			done = false;
		}
		else if(input == 2){
			drop();
			done = false;
		}
	}
	
	public void pickUp(){
		String card;
		
		card = Deck.drawCard();
		System.out.println(card);
		
		Hand.addCard(card);
	}
	
	public void drop(){
		int input;
		
		System.out.print("Which card would you like to drop?(1 for first card, 2 for second card, etc.): ");
		input = scan.nextInt();
		
		input = input - 1;
		
		Hand.dropCard(input);
	}
	
	public void displayHand(){
		Hand.displayHand();
	}
	
	public static boolean completeTurn(){
		if(sumOrProduct())
			return true;
		else
			return false;
	}
}




import java.util.Random;
import java.util.Scanner;

public class Hand {

	static String hand[];
	static int target;
	Random rng = new Random();
	static Scanner scan = new Scanner(System.in);
	
	public Hand(){
		hand = new String[5];
		target = rng.nextInt(40) + 10;
	}
	
	public static void displayHand(){
		if(isEmpty())
			System.out.println("Player has an empty hand.");
		switch(hand.length){
			case 1:
				System.out.println("Player has a hand of: {" + hand[0] + "}");
				break;
			case 2:
				System.out.println("Player has a hand of: {" + hand[0] + "," + hand[1] + "}");
				break;
			case 3:
				System.out.println("Player has a hand of: {" + hand[0] + "," + hand[1] + "," + hand[2] + "}");
				break;
			case 4:
				System.out.println("Player has a hand of: {" + hand[0] + "," + hand[1] + "," + hand[2] + "," + hand[3] + "}");
				break;
			case 5:
				System.out.println("Player has a hand of: {" + hand[0] + "," + hand[1] + "," + hand[2] + "," + hand[3] + "," + hand[4] + "}");
				break;
			default:
				System.out.println("Error");
				break;
		}
	}
	
	public static boolean isEmpty(){
		if(hand.length == 0)
			return true;
		else
			return false;
	}
	
	public static boolean isFull(){
		if(hand.length == 5)
			return true;
		else
			return false;
	}
	
	public static void addCard(String input){
		for (int i = 0; i < hand.length; i++){
			if(hand[i] == null)
				hand[i] = input;
		}
	}
	
	public static void dropCard(int i){
		String temp;
		
		temp = hand[hand.length];
		hand[hand.length] = hand[i];
		hand[i] = temp;
	}
	
	public static int aceValue(){
		int input;
				
		System.out.print("You have an ace. Would you like it count as a 1 or an 11: ");
		input = scan.nextInt();	
		
		if(input == 1)
			return input;
		
		if(input == 11)
			return input;
		
	}
	
	public int containsCard(String input){
		for(int i = 0; i < hand.length; i++){
			if(hand[i].equals(input))
				return i;
		}
		return -1;
	}
	
	public static boolean sumAndProduct(){
		int sum = 0;
		int product = 1; 
		int input;
		
		for(int i = hand.length; i > 0; i--){
			switch(hand[i]){
			case "A":
				sum += aceValue();
				break;
			case "2":
				sum += 2;
				break;
			case "3":
				sum += 3;
				break;
			case "4":
				sum += 4;
				break;
			case "5":
				sum += 5;
				break;
			case "6":
				sum += 6;
				break;
			case "7":
				sum += 7;
				break;
			case "8":
				sum += 8;
				break;
			case "9":
				sum += 9;
				break;
			case "10":
			case "J":
			case "Q":
			case "K":
				sum += 10;
				break;
			default:
				System.exit(0);
				break;
			}

		for(int i = hand.length; i > 0; i--){
			switch(hand[i]){
			case "A":
				product *= aceValue();
				break;
			case "2":
				product *= 2;
				break;
			case "3":
				product *= 3;
				break;
			case "4":
				product *= 4;
				break;
			case "5":
				product *= 5;
				break;
			case "6":
				product *= 6;
				break;
			case "7":
				product *= 7;
				break;
			case "8":
				product *= 8;
				break;
			case "9":
				product *= 9;
				break;
			case "10":
			case "J":
			case "Q":
			case "K":
				product *= 10;
				break;
			default:
				System.exit(0);
				break;
			
			if(sum == target || product == target){
				return true;
			}
			
	    }
	}
}





import java.util.Scanner;

public class Game {

	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		
		String name1;
		String name2;
		boolean done = false;
				
		System.out.print("Player 1, please enter your name: ");
		name1 = scan.nextLine();
		System.out.print("Player 2, please enter your name: ");
		name2 = scan.nextLine();
		
		Player player1 = new Player(name1);
		Player player2 = new Player(name2);
		
		do{
			if(player1.takeTurn()){
				done = true;
			}
			if(player2.takeTurn()){
				done = true;
			}
		}while(!done);

		if(player1.completeTurn())
			System.out.println("Congrats " + player1.getName() + "! You won!");
		else if(player2.completeTurn())
			System.out.println("Congrats " + player2.getName() + "! You won!");
		else if(player1.completeTurn() && player2.completeTurn())
			System.out.println("Congrats " + player1.getName() " and " + player2.getName() + "! You tied!");

	}

}





import java.util.Random;

public class Deck {

	static String deck[] = {"A", "A", "A", "A", "2", "2", "2", "2", "3", "3", "3", "3", "4", "4", "4", "4", "5", "5", "5", "5", "6", "6", "6", "6", "7", "7", "7", "7", "8", "8", "8", "8", "9", "9", "9", "9",
			"10", "10", "10", "10", "J", "J", "J", "J", "Q", "Q", "Q", "Q", "K", "K", "K", "K",};
	static String scrapDeck[];
	static int numOfCards = 52;
	
	public Deck() {
		scrapDeck = new String[numOfCards];
		shuffle();
	}
	
	public static void shuffle(){
		Random generator = new Random();
		int index1;
		int index2;
		String temp;
		
		for (int i = 0; i < 3; i++){
			index1 = generator.nextInt(numOfCards);
			index2 = generator.nextInt(numOfCards);
			
			temp = deck[index1];
			deck[index1] = deck[index2];
			deck[index2] = temp;
		}
	}
	
	public static boolean isEmpty(){
		if (deck.length == 0)
			return true;
		else
			return false;
	}
	
	public static String drawCard(){
		if(isEmpty()){
			swapDecks();
			shuffle();
		}
		return deck[0];
	}
	
	public static void swapDecks(){
		for (int i = 0; i < scrapDeck.length; i++){
			deck[i] = scrapDeck[i];
		}
		
		shuffle();
	}
	
	public void insertCard(String input){
		int index = 0;
		
		scrapDeck[index] = input;
	}
}
