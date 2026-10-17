Day 3 Password Failed Logger   Recap


 I have been give 3 questions at the start of today.
1. What is in your test file and why did you build it that way?
2. What's the difference between grep "Failed password" and grep -c "Failed password"?
3.  Why does counting from the end of the line fix the "invalid user" problem.
Question 1.
	My test file is pretty much logging the failed attempts to get into a certain account in this case it is john123. The first 2 Attempts are for john123. These are both from the same IP address. This is possibly from a user that has fat-fingered the account and forgotten there password or it could be a malicious person trying to get access into the account.
	The next couple of lines (2) are 2 password failed messages for an invalid user. In this case I think I used pig432 or something and Tom123 for the fake accounts again these are from the same IP to demonstrate that someone could be trying to find an account to try and brute force there way in.
	The next line is of a successful login, highlited by the fact that it has accepted the public key and user has access into the system. We can also corralate this to the first 2 lines. I could mean that the person forgot there password, and then finally got in or it could mean that the intruder managed to brute force there way in.
	The last line is of nothing to do with passwords or anything of the sort, This is just a message of a user disconnected from the server or system.


Question 2.

	The difference between the 2 are that grep "word word word" will find any line of code in that selected file and print it as it is. While with the -c (—count) command it is going not print the total lines of code out only print what it has found.
									Example: word word word                                       											Example: 4
											word word word

Question 3.
	Counting backwards helps a lot better because the last 4 of any access, failed password and or invalid user log will always have a IP port and the ssh2 at the end. So if we counted from the back in most cases it will always print 4, to capture the IP. If we counted forwards (normal counting) we would hit an error or something we are not looking for. So that's why counting backwards is always better if we are wanting to capture the IP or the port as it will always be in the same space.





Question 4.
	Which two words are in the invalid-user line that aren't in the normal failed-password line, and where do they sit? One sentence.
	The answer - "invalid-User"
	Explanation - In a normal failed password line, and is for the correct & valid user. There would not be invalid-user text but if it was an invalid user it would pop up and it would sit after Failed password for…..


Day 3


What does it print, and how many lines of output?
		 awk '{print $1}' test_auth.log


Prediction- In my understanding I think that it will print the likes of line 1, which is in our case the month. Or a smaller abbreviation of the month. So in our case it will be "Aug". It will print 6 lines of "Aug"

I ran the command.


Input -  awk '{print $1}' test_auth.log
Output -
		user@mac password-test % awk '{print $1}' test_auth.log
		Aug
		Aug
		Aug
		Aug
		Aug
		Aug
		user@mac password-test %


AWK research.

What do I need to research?
1. awk has a built-in variable that holds the number of fields on a line. Find its name. It's two or three capital letters.
2. what that variable is called, an in your own words, what $NF would print?

Source - https://ftp.gnu.org/old-gnu/Manuals/gawk/html_node/Fields.html

AWK splits each input record into seperate fields, usually using spaces or tabs. Fields make it easier to work with specific parts of a line instead of the whole record. The $ symbol followed by a number is used to access a field, such as $1 for the first field and $2 for the second. $0 represents the entire record, while NF stores the total number of fields and $NF refers to the last field. If you try to access a field that doesnt exist, AWK returns an empty string or zero in numeric operations. Punctuation attached to a word is included in that field. AWK can also search for matching text using the ~ operator and regular expressions, allowing you to check specific fields or the whole record and print the information needed.


Command: awk '{print $NF}' test_auth1.log

Output: ssh2
