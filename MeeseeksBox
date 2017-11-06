package Players.MeeseeksBox;

import Interface.PlayerModulePart1;
import Interface.PlayerModulePart2;
import Interface.PlayerModulePart3;
import Interface.PlayerMove;
import Interface.Coordinate;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

/**
 * MeeseeksBox player for RIT CS II Project 2. This player implimements the playerModules from
 * part1, part2, and part3.
 * @author Mohammed Nafiuzzaman < mxn4459@cs.rit.edu >
 */
public class MeeseeksBox implements PlayerModulePart1, PlayerModulePart2, PlayerModulePart3 {

	/** id for this player to use (1 or 2) **/
	private int id;
	/** id of the player who made the last move **/
	private int lastPlayerID;
	/** two dimensional array representation of the board. 0 for empty, 1 for player 1, and 2 for player 2 **/
	private int[][] board;

	/**
	 * Method called to initialize a player module. Required task for Part 1.
	 * Note that for tournaments of multiple games, only one instance of each PlayerModule is created.
	 * The initPlayer method is called at the beginning of each game, and must be able to reset the
	 * player for the next game.
	 *
	 * @param dim - size of the smaller dimension of the playing area for one player. The grid of nodes for that player is of size dim x (dim+1).
	 * @param id - id (1 or 2) for this player.
	 */
	@Override
	public void initPlayer(int dim, int id) {
		this.id = id;
		this.board = new int[(2*dim)+1][(2*dim)+1]; // establishes the board of proper size (default filled with all 0's

		// fills the array
		for(int row=0; row < board.length; row++) {
			for(int col=0; col < board.length; col++) {
				if(isSpace(new Coordinate(row, col)) && (col == 0 || col == board.length-1)) board[row][col] = 1; 		// Fills left and right side of board with all player 2 values so the end is fully connected
				else if(isSpace(new Coordinate(row, col)) && (row == 0 || row == board.length-1)) board[row][col] = 2;	// Fills top and bottom side of board with all player 2 values so the end is fully connected
				else if(isPlayerOne(new Coordinate(row, col))) board[row][col] = 1; // fills in the player 1 spots
				else if(isPlayerTwo(new Coordinate(row, col))) board[row][col] = 2; // fills in the player 2 spots
			}

		}
	}

	/**
	 * Private helper method that determines if a coordinate should be a player 1 location at start
	 *
	 * @param coord - is the coordinate to check
	 * @return true if it belongs to player 1
	 */
	private boolean isPlayerOne(Coordinate coord) {
		return (coord.getRow()%2 != 0 && coord.getCol()%2 == 0); // odd row and even column
	}

	/**
	 * Private helper method that determines if a coordinate should be a player 2 location at start
	 *
	 * @param coord - is the coordinate to check
	 * @return true if it belongs to player 2
	 */
	private boolean isPlayerTwo(Coordinate coord) {
		return (coord.getRow()%2 == 0 && coord.getCol()%2 != 0); // even row and odd column
	}

	/**
	 * Private helper method that determines if a coordinate should be a space location at start
	 *
	 * @param coord - is the coordinate to check
	 * @return true if it belongs to neither player 1 or player 2
	 */
	private boolean isSpace(Coordinate coord) {
		return (!isPlayerOne(coord) && !isPlayerTwo(coord)); // not a player 1 and not a player 2, must be a space
	}

	/**
	 * Private helper method that tells you if the id at a cord is equal to a given id
	 *
	 * @param coord - is the coordinate to check
	 * @return true if it belongs to the given id
	 */
	private boolean equalsID(Coordinate coord, int id) {
		return board[coord.getRow()][coord.getCol()] == id;
	}

	/**
	 * Method called after every move of the game. Used to keep internal game state current. Required task for Part 1.
	 * Note that the engine will only call this method after verifying the validity of the current move. Thus, you do
	 * not need to verify the move provided to this method. It is guaranteed to be a valid move.
	 *
	 * @param move - PlayerMove representing the most recent move
	 */
	@Override
	public void lastMove(PlayerMove move) {
		lastPlayerID = move.getPlayerId(); // updates the id, so we know who made the last move
		board[move.getCoordinate().getRow()][move.getCoordinate().getCol()] = move.getPlayerId(); // error handling is checked before call and just puts the id from this move at the coordinate from this move.
	}

	/**
	 * Part 1 task that tests if a player has won the game given a set of PREMOVEs.
	 *
	 * @param id - player to test for a winning path.
	 * @return boolean value indicating if the player has a winning path.
	 */
	@Override
	public boolean hasWonGame(int id) {
		if(id == 1) return canReachBFS(new Coordinate(0, 0), new Coordinate(board.length-1, board.length-1), 1); // runs BFS for player 1. tries connect from top-left to bottom-right because both sides are completely connected
		return canReachBFS(new Coordinate(0, 1), new Coordinate(board.length-1, board.length-2), 2); // runs BFS for player 2. tries connect from top-left to bottom-right because both sides are completely connected
	}

	/**
	 * Method that visits all nodes reachable from the given starting coordinate
	 * in breadth-first search fashion using a queue, stopping only if the
	 * finishing node is reached or the search is exhausted. A predecessors map
	 * keeps track of which coordinates have been visited and along what path
	 * they were first reached.
	 *
	 * Precondition: the inputs correspond to coordinates in the board.
	 *
	 * @param start - the coordinate associated with the coordinate from which to start the search
	 * @param end - the coordinate associated with the destination coordinate
	 * @return path the path from start to finish. Empty if there is no such path.
	 */
	private boolean canReachBFS(Coordinate start, Coordinate end, int id)  {
		List<Coordinate> queue = new ArrayList<>();
		Set<Coordinate> visited = new HashSet<>();

		// prime the queue with the starting coordinate
		queue.add(start);
		// put the starting coordinate in, and just assign itself as predecessor
		visited.add(start);

		// loop until either the finish coordinate is found, or the dispenser is empty (no path)
		while(!queue.isEmpty()) {
			Coordinate current = queue.remove(0);
			if(current.equals(end)) return true; // found path
			// loop over all neighbors of current
			for(Coordinate nbr : getNeighbors(current, id)) {
				// process unvisited neighbors
				if(!visited.contains(nbr)) {
					visited.add(nbr);
					queue.add(nbr);
				}
			}
		}
		return false;
	}

	/**
	 * Helper method that returns the neighbors around the given coordinate
	 *
	 * @param coord - is the coordinate to look at
	 * @param id - is the id to compare is associated at current coordinate
	 * @return ArrayList of coordinates that are the neighbors at current location
	 */
	private ArrayList<Coordinate> getNeighbors(Coordinate coord, int id) {
		int row = coord.getRow();
		int col = coord.getCol();
		ArrayList<Coordinate> neighbors = new ArrayList<>();

		// checks to make sure it is in bounds and is the matching id, if so adds it to the neighbors list
		if(row+1 < board.length && board[row+1][col] == id) neighbors.add(new Coordinate(row+1, col));  // left
		if(row-1 > 0 && board[row-1][col] == id) neighbors.add(new Coordinate(row-1, col));				// right
		if(col+1 < board.length && board[row][col+1] == id) neighbors.add(new Coordinate(row, col+1));  // top
		if(col-1 > 0 && board[row][col-1] == id) neighbors.add(new Coordinate(row, col-1));				// bottom
		return neighbors;
	}

	/**
	 * Helper method that finds all the neighbors at the top, bottom, right, left of the given coordinate
	 * which are of the same player type or a space
	 *
	 * @param coord - is the coordinate to look at
	 * @param id - is the id to compare is associated at current coordinate
	 * @return ArrayList of coordinates that are the neighbors at current location
	 */
	private ArrayList<Coordinate> getNeighborsWithSpaces(Coordinate coord, Integer id) {
		int row = coord.getRow();
		int col = coord.getCol();

		ArrayList<Coordinate> neighbors = new ArrayList<>();
		// checks if it matches the given id or is a space
		if(row !=board.length-1 && (board[row+1][col] == 0 || board[row+1][col] == id)) neighbors.add(new Coordinate(row+1, col));     //right
		if(row!=0 && (board[row-1][col] == 0 || board[row-1][col] == id)) neighbors.add(new Coordinate(row-1, col));                   // left
		if(col!=0 && (board[row][col-1] == 0 || board[row][col-1] == id)) neighbors.add(new Coordinate(row, col-1));                   // bottom
		if(col != board.length-1 && (board[row][col+1] == 0 || board[row][col+1] == id)) neighbors.add(new Coordinate(row, col+1));    // top
		return neighbors;
	}

	/**
	 * Dijkstra Algorithm to calculate the lowest cost path and returns a list of moves to make that will complete this path
	 *
	 * @param id - The id of the player
	 * @return List of all possible moves.
	 */
	private ArrayList<Coordinate> dijkstra(Coordinate start, Coordinate end, int id) {
		ArrayList<Coordinate> priorityQ = new ArrayList<>();
		HashMap<Coordinate, Coordinate > predecessors = new HashMap<>();
		HashMap<Coordinate, Integer> distance  = new HashMap<>();

		// fills any spot containing the given id or a space with "infinity"
		for(int r=0; r < board.length; r++) {
			for(int c=0; c < board.length; c++) {
				Coordinate curr = new Coordinate(r, c);
				if(equalsID(curr, id) || isSpace(curr)) {
					priorityQ.add(curr);
					distance.put(curr, Integer.MAX_VALUE);
				}
			}
		}

		distance.put(start, 0); // setting the distance of the start coordinate 0
		predecessors.put(start,start); // adds the start and points to itself to mark the beginning point

		while (!priorityQ.isEmpty()) {
			Coordinate U = dequeueMin(priorityQ, distance); // gets the lowest node

			// return if this node still has distance "infinity" -
			// remaining nodes are inaccessible
			if(distance.get(U) == Integer.MAX_VALUE) break;

			// this loop allows neighbors that have already been finalized
			// to be checked again, but they will never be updated and
			// this doesn't affect overall complexity
			ArrayList<Coordinate> neighbors = getNeighborsWithSpaces(U, id); // gets the neighbors
			for(Coordinate x : neighbors) {
				int weight = 1; // starts weight at 1 for spaces
				if(board[x.getRow()][x.getCol()] == id) weight = 0; // if it is an id node the cost is 0
				// relaxation
				Integer distViaU = distance.get(U) + weight; // adds together the total weight to get to this point
				// updates
				if(distance.get(x) > distViaU) {
					distance.put(x,  distViaU);
					predecessors.put(x,  U);
				}
			}
		}

		// puts all the moves made to get to this shortest path (lowest cost) into an array tha represents this path
		Coordinate i = end; // starts at end
		ArrayList<Coordinate> allMoves = new ArrayList<>();
		if(!predecessors.containsKey(end)) return allMoves;
		while(i != start) {
			allMoves.add(i);
			i = predecessors.get(i); //go through the predecessors hash map and go to the start coordinate
		}

		// puts all the spaces that need to be made to do this shortest path (lowest cost) into an array of moves that need to be made
		ArrayList <Coordinate> possibleMoves = new ArrayList<>();
		for(Coordinate z : allMoves)
			if(board[z.getRow()][z.getCol()] == 0) possibleMoves.add(z);

		return possibleMoves; // returns an array of the moves needed to be made for the calculated shortest path
	}

	/**
	 * This method will dequeue the minimum
	 *
	 * @param priorityQ - the priority Q
	 * @param distance - distance from one to other
	 * @return the minimum
	 */
	private Coordinate dequeueMin(List<Coordinate> priorityQ, Map<Coordinate, Integer> distance) {
		Coordinate minNode = priorityQ.get(0);  // start off with first one
		for(Coordinate n : priorityQ) // checks first one again...
			if(distance.get(n) < distance.get(minNode)) minNode = n; // updates min node with lowest value
		return priorityQ.remove(priorityQ.indexOf(minNode));
	}

	/**
	 * Part 2 task that tests if a player can correctly generate all legal moves,
	 * assuming that it is that player's turn and given the current game status.
	 *
	 * Preconditions: you may assume that there is no winner yet based on prior moves.
	 * You may also assume that this method will only be called when it is actually
	 * your player's turn based on prior moves.
	 *
	 * @return a List of all legal PlayerMove objects. They do not have to be in any particular order.
	 */
	@Override
	public List<PlayerMove> allLegalMoves() {
		ArrayList<PlayerMove> legalMoves = new ArrayList<>();
		for(int r=1; r < board.length-1; r++) {
			for(int c=1; c < board.length-1; c++) {
				Coordinate cord = new Coordinate(r,c);
				if(isSpace(cord)) legalMoves.add(new PlayerMove(cord, id)); // all spaces are legal moves
			}
		}
		return legalMoves;
	}

	/**
	 * Part 2 task that computes the fewest segments that a given player needs to add to complete a winning path.
	 * This ignores the possibility that the opponent might block the path.
	 *
	 * Precondition: you may assume that the other player has not already won the game. That is, you may assume
	 * that a winning path still exists for the player of interest.
	 *
	 * @param playerId - the player of interest
	 * @return the fewest segments for victory
	 */
	@Override
	public int fewestSegmentsToVictory(int playerId) {
		// returns the size of the array returned by Dijkstra Algorithm, which is the total number of moves to be made for the shortest path
		return (playerId == 1) ? dijkstra(new Coordinate(0, 0), new Coordinate(board.length-1, board.length-1), 1).size() :
				dijkstra(new Coordinate(0, 1), new Coordinate(board.length-1, board.length-2), 2).size();
	}

	@Override
	public PlayerMove move() {
		// invalidation check. the other player is invalid so always make the offensive move
		if(lastPlayerID == this.id)
			return (this.id == 1) ?
					new PlayerMove(dijkstra(new Coordinate(0, 0), new Coordinate(board.length-1, board.length-1), 1).get(0), this.id) :
					new PlayerMove(dijkstra(new Coordinate(0, 1), new Coordinate(board.length-1, board.length-2), 2).get(0), this.id);
			// makes the move based on whoever is fewestSegmentsToVictory. This means that if the other player is closer it will make a defensive move
			// and if this player is closer it will make the offensive move.
		else if(this.id == 1) // if MeeeseeksBox is player one
			return (fewestSegmentsToVictory(1) < fewestSegmentsToVictory(2)) ?
					new PlayerMove(dijkstra(new Coordinate(0, 0), new Coordinate(board.length-1, board.length-1), 1).get(0), this.id) :
					new PlayerMove(dijkstra(new Coordinate(0, 1), new Coordinate(board.length-1, board.length-2), 2).get(0), this.id);
		else // if MeeeseeksBox are player two
			return (fewestSegmentsToVictory(2) < fewestSegmentsToVictory(1) ) ?
					new PlayerMove(dijkstra(new Coordinate(0, 1), new Coordinate(board.length-1, board.length-2), 2).get(0), this.id) :
					new PlayerMove(dijkstra(new Coordinate(0, 0), new Coordinate(board.length-1, board.length-1), 1).get(0), this.id);
	}

	/**
	 * Part 3 task that computes whether the given player is guaranteed with optimal strategy to have won the
	 * game in no more than the given number of total moves, also given whose turn it is currently.
	 *
	 * PRECONDITION: you may assume that numMoves is non-negative.
	 *
	 * @param playerId - player to determine winnable status for
	 * @param whoseTurn - player whose turn it is currently
	 * @param numMoves - num of total moves by which the player of interest must be able to guarantee victory to satisfy the requirement to return a value of true
	 * @return boolean indicating whether it is possible for the indicated player to guarantee a win after the specified number of total moves.
	 */
	@Override
	public boolean isWinnable(int playerId, int whoseTurn, int numMoves) {
		int fewestMoves = fewestSegmentsToVictory(playerId)*2; // determines this players fewestMoves and multiplied by 2 to account for the other players moves as well

		if(fewestMoves == 0 && playerId == whoseTurn) return false; // this means the last move was the other player, which means the other player won the game
		if(fewestMoves == 0) return true; // the given playerId has already won
		if(numMoves == 0) return false; // there is no possibility for this player to win
		if(whoseTurn == playerId) fewestMoves -= 1; // if the current player has the turn, then the total number of moves that it need to make is one less
		if(numMoves < fewestMoves) return false; // the number of moves that can be made is less than the number needed.
		if(numMoves == 1 && whoseTurn != playerId) return false;
		if(fewestMoves == 1 && whoseTurn == playerId) return true;

		int next = (whoseTurn == 1) ? 2 : 1; // if its player 1's turn, the next will be player 2, or vice versa.
		// generate successors
		for(int row = 0; row < board.length; row++) {
			for(int column = 0; column < board.length; column++) {
				if(board[row][column] == 0) {
					board[row][column] = whoseTurn; // setting the empty space to the whose turn's player id
					boolean x = isWinnable(playerId, next, numMoves-1); // recursive call with this successors
					board[row][column] = 0; // undoes the modification to the board
					if(x && whoseTurn == playerId) return true;
					if(!x && whoseTurn != playerId) return false;
				}
			}
		}

		return (playerId != whoseTurn);
	}

	@Override
	public void otherPlayerInvalidated() {
		// do nothing
	}

}
