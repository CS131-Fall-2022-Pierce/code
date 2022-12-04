# code
cpp code
#include <iostream>
#include <string>
using namespace std;

string LongestCommonSubstring(const string& str1, const string& str2) {
   // Allocate the matrix
   int matrix[str1.length()][str2.length()];
      
   // Variables to remember the largest matrix element's value and position
   int maxValue = 0;
   int maxValueRow = 0;
   int maxValueCol = 0;
   for (int row = 0; row < str1.length(); row++) {
      for (int col = 0; col < str2.length(); col++) {
         // Check if the characters match
         if (str1[row] == str2[col]) {
            // Get the value in the cell that's up and to the 
            // left, or 0 if no such cell
            int upLeft = 0;
            if (row > 0 && col > 0) {
               upLeft = matrix[row - 1][col - 1];
            }
                
            // Set the value for this cell
            matrix[row][col] = 1 + upLeft;
            if (matrix[row][col] > maxValue) {
               maxValue = matrix[row][col];
               maxValueRow = row;
               maxValueCol = col;
            }
         }
         else {
            matrix[row][col] = 0;
         }
      }
   }

   // Return the longest common substring
   int startIndex = maxValueRow - maxValue + 1;
   return str1.substr(startIndex, maxValue);
}
   
string LongestCommonSubstringOptimized(const string& str1, const string& str2) {
   // Create one row of the matrix
   int matrixRow[str2.length()];

   // Variables to remember the largest matrix element's value and row index
   int maxValue = 0;
   int maxValueRow = 0;
   for (int row = 0; row < str1.length(); row++) {
      // Variable to hold the upper-left value from the
      // current matrix position
      int upLeft = 0;
      for (int col = 0; col < str2.length(); col++) {
         // Save the current cell's value; this will be upLeft
         // for the next iteration
         int savedCurrent = matrixRow[col];
        
         // Check if the characters match
         if (str1[row] == str2[col]) {
             matrixRow[col] = 1 + upLeft;
                
             // Update the saved maximum value and row, if appropriate
             if (matrixRow[col] > maxValue) {
                maxValue = matrixRow[col];
                maxValueRow = row;
             }
         }
         else {
            matrixRow[col] = 0;
         }
                
         // Update the upLeft variable
         upLeft = savedCurrent;
      }
   }

   // The longest common substring is the substring in str1 starting at index 
   // maxValueRow - maxValue + 1 and having maxValue characters
   int startIndex = maxValueRow - maxValue + 1;
   return str1.substr(startIndex, maxValue);
}

// Main program to test the longest common substring algorithms
int main() {
   string firstString, secondString;
   cout << "Enter the first string: ";
   getline(cin, firstString);
   cout << "Enter the second string: ";
   getline(cin, secondString);
   cout << endl;
      
   string unoptimized = LongestCommonSubstring(firstString, secondString);
   string optimized = LongestCommonSubstringOptimized(firstString, secondString);
   cout << endl;
   cout << "Unoptimized solution: " << unoptimized << endl;
   cout << "Optimized solution: " << optimized << endl;
}
