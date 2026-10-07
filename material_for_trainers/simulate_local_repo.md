echo "--- Solve assignment to create local repo ---"

cd Desktop/
mkdir weather-notes
cd weather-notes/
git init
touch README.md
touch LICENSE
git add README.md LICENSE 
git commit -m "Initial commit: add README and LICENSE"
echo "The sun came out"
echo "The sun came out" > notes.txt
git add notes.txt 
git commit -m "Add first line"
echo "The air was fresh" >> notes.txt 
git add notes.txt 
git commit -m "Add second line"
echo "A cloudy afternoon" >> notes.txt 
git add notes.txt 
git commit -m "Add third line"
echo "*.log" > .gitignore
echo "data/" >> .gitignore
git add .gitignore
git commit -m "Ignore all log and data files"
git add remote origin git@github.com:[your-user-name]/weather-notes.git
git remote add origin git@github.com:[your-user-name]/weather-notes.git
git push origin main

echo "--- New commands for branching ---"
git branch b1
git switch b1
git switch -c b2
git switch main

echo "--- Develop on different branches  ---"
echo "A dramatic sunset" >> notes.txt 
git add notes.txt 
git commit -m "Add fourth line"
git switch b1
echo "A dramatic sunset" >> notes.txt 
git add notes.txt 
git commit -m "Add fourth line on branch b1"
git switch main
git switch b2 
echo "A dramatic sunset" >> notes.txt 
echo "The moon was bright" >> notes.txt 
git add notes.txt 
git commit -m "Add two more lines on b2"

echo "--- Explore differences across branches ---"
git switch main
echo "A dramatic sunset (duplicate)" >> notes.txt
git add notes.txt 
git commit -m "Add fifth line on main (with mistake)" notes.txt
git switch b1
echo "The moon was bright" >> notes.txt 
git add notes.txt 
git commit -m "Add fifth line on b1" notes.txt 

echo "--- Merging branches and conflict resolution ---"
git switch b1
echo "It rained at night" >> notes.txt 
echo "Give me my thick blanket" >> notes.txt
git add notes.txt
git commit -m "Add sixth and seventh lines on b1"
git switch main
git merge -X theirs b1 -m "Merge changes from b1 into main"

echo "--- A first type for merge ---"
git switch main
git merge -X ours b2 -m "Merge b2 into main"
