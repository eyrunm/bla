
package Percolation;

import edu.princeton.cs.algs4.StdIn;
import edu.princeton.cs.algs4.QuickFindUF;
import edu.princeton.cs.algs4.WeightedQuickUnionUF;

public class Percolation {


		// the 2-by-2 dimensional grid
		private boolean[][] grid;
		// the size of the grid
		private int N;
		// to count the number of open sites
		int count;
		// union find
		private QuickFindUF uf;
		private WeightedQuickUnionUF wuf;
		private int top, bottom;
		
		// create N-by-N grid, with all sites initially blocked
		public Percolation(int N) {
			
			this.N = N;
			
			if(N <= 0)
				throw new java.lang.IllegalArgumentException();
			
			//create a new grid with N-by-N size
			grid = new boolean[N][N];
			// create a new WeightedUF with N * N + 2 virtual top and bottom sites
			wuf = new WeightedQuickUnionUF(N * N + 2);
			top = 0;
			bottom = N * N + 1;
			count = 0;
		}
			
	
		
		// open the site (row, col) if it is not already open
		public void open(int row, int col) {
			
			outOfBounds(row, col);
			// if site is not open
			if(isOpen(row, col) ==  false) {
				// open the site
				grid[row][col] = true;
				// increment the number of open sites
				count++;
				// link to neighbors sites
				checkIfConnected(row, col);
			}
		}
		
		// is the site (row, col) open?
		public boolean isOpen(int row, int col) {
			// check if out of bounds
			outOfBounds(row, col);
			
			// else return true/false for row, col
			return grid[row][col];
		}
		
		// is the site (row, col) full?
		public boolean isFull(int row, int col) {
			outOfBounds(row, col);
			
			if(wuf.connected(row, col) == true)
				return true;
			
			return false;
		}
		
		// number of open sites
		public int numberOfOpenSites() {
			for(int i = 0; i < N; i++)
				for(int j = 0; j < N; j++)
					if(isOpen(i, j))
						count++;
			
			return count;
		}
		
		// does the system percolate?
		public boolean percolates() {
			// checks if the virtual top 0 is connected to the virtual bottom N * N + 1
			return wuf.connected(0, N * N + 1);
		}
		
		// helper function to check if sites are connected
		private void checkIfConnected(int row, int col) {
			// top case = row - 1, col
			if(isOpen(row - 1, col)) {
				int i = xyTo1D(row, col);
				int j = xyTo1D(row - 1, col);
				wuf.union(i, j);
			}
			
			// bottom case = row + 1, col
			if(isOpen(row + 1, col)) {
				int i = xyTo1D(row, col);
				int j = xyTo1D(row + 1, col);
				wuf.union(i, j);
			}
			
			// right case = row, col + 1
			if(isOpen(row, col + 1)) {
				int i = xyTo1D(row, col);
				int j = xyTo1D(row, col + 1);
				wuf.union(i, j);
			}
			
			// left case = row, col - 1
			if(isOpen(row, col - 1)) {
				int i = xyTo1D(row, col);
				int j = xyTo1D(row, col - 1);
				wuf.union(i, j);
			}
		}
		
		private int xyTo1D(int row, int col) {
			return (row * N + col);
			
		}
		
		
		private void outOfBounds(int row, int col) {
			if(!isValid(row, col))
				throw new java.lang.IndexOutOfBoundsException();
		}
		
		//returns true if site is valid
		private boolean isValid(int row, int col) {
			return (row > 0 && row < N && col > 0 && col < N);
		}

		// perform unit-testing in main
		public static void main(String[] args) {
			// TODO Auto-generated method stub
			Percolation perc = new Percolation(2);
			perc.open(1, 1);

		}

	}
