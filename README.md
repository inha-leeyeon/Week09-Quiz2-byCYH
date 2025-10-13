# Week05 Quiz2:

## 1. Requirements:

A local government has introduced a newborn bonus program—each family will receive 100g of 24K gold bars engraved with their baby's name, birth date, 100th day(i.e. On the 100th day of birth), first birthday, and the scheduled start date for elementary school (i.e., March 1st of the year after seven years of birth). Make a system manages newborn information through a database of babys

- Writing code using c++
- Use `Baby` class and `Date` class to complete your code. 
- Changing the provided class is not allowed. You can `only use the provided class and the functions defined within it`.
- You need to store the `two given class definitions` in the `Baby.h` and `Date.h` files respectively. Store the `function definitions and the main function` in the `main.cpp` file.   
  `(Please submit the above three documents, and if you do not name the main file "main.cpp", the test will not pass and you will receive zero points)`
- Please confirm the containment relationship between classes and ensure reasonable calls between header files.

<br/>
                  
### Baby class:
- Complete and use the following class:
  ```C++
  class Baby {
	public:
		Baby(const std::string & = "", const Date & = Date());
  
		void setName(string&); // set the value of its parameters to the name data(string type)
  
		void setBirthDate(Date&);
  		// set the value of its parameters to the birthDate data(Date type), If the baby is born on February 29 in a leap year, their first birthday is rescheduled to February 28 of the following year.
  
		string getName() const; // return the value of name data.
  
		Date getBirthDate() const; // return the value of birthDate data.
  
	private:
		string name;
		Date birthDate;
	};
  ```
<br/>

### Date class
- Complete and use the following class:
  ```C++
  class Date {
		friend std::ostream& operator<<(std::ostream&, const Date&); //
		friend Date firstbirthday(const Date&);
	public:
		Date(int m = 1, int d = 1, int y = 1900); // default constructor, The provided data is raw data in the format of "month, day, year".
  
		void setDate(int, int, int); // Input sequence should be `month, day, year`. The input of year should `between 1900 and 2100`.
  
		Date& operator++(); // prefix increment operator
  
		Date operator++(int); // postfix increment operator
  
		Date& operator+=(unsigned int); // add days, modify object
  
		static bool leapYear(int); // verify that if the year a leap year, return true when correct.
  
		bool endOfMonth(int) const; // verify that if the input day is the last day of month, return true when correct.
  
		int schoolyear() const; // return the year baby goto the primary school.It should be the 7th year after born (this.year += 7).
  
	private:
		unsigned int month;
  
		unsigned int day;
  
		unsigned int year;
  
		int days[13] = { 0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 }; // maximum days per month. In a leap year, February has 29 days.
  
		void helpIncrement();
  		// utility function for incrementing date. If today is the last day of month, then set day = 1 and month += 1, If today is the last day of year, then set day = 1, month = 1 and year += 1.
	};
  ```
- The input of year should `between 1900 and 2100`.
- The input of month should `between 1 and 12`.
- The input of day should `greater than or equal to 1 and less than the maximum number of days in this month`.
- If not follow above rules, an error will occur while creating a Date data, output the error message and use the default value of `January 1, 1900`.
- Leap year: Years that can be divided by 4 but not by 100, or years that can be divided by 400.
  ```C++
  bool leap;
  if ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0))
  leap = true;
  else
  leap = false;
  ```
- `operator++()` function is used to overload prefix increment operator.
  ```C++
  Date& Date::operator++() {
	  helpIncrement(); // increment date 
	  return *this; // reference return to create an lvalue
  }
  ```
- `operator++(int)` function is used to overload postfix increment operator.
  ```C++
  Date Date::operator++(int) {
  	Date temp = *this ; // hold current state of object 
  	helpIncrement();
  
  	// return unincremented, saved, temporary object 
  	return temp;  // value return; not a reference return 
  }
  ```
- `operator+=(unsigned int additionalDays)` function is used to add specified number of days to date by overload the operator '+='.
  ```C++
  Date& Date::operator+=(unsigned int additionalDays) {
  	for (unsigned int i = 0; i < additionalDays; ++i) {
  		helpIncrement();
  	}
  	return *this; // enables cascading 
  }
  ```
- `operator<<` function is used to directly output birthday on a form of `month/day/year` by overload the operator '<<'.
  ```C++
  ostream& operator<<(ostream& output, const Date& d) {
  	
  	output << d.month << "/" << d.day << "/" << d.year;
  	return output; // enables cascading
  }
  ```
<br/>

### Input
- The first line contains a number (int type) representing the number of babies that are about to be entered.
- Starting from the second line, input the corresponding number of baby's data based on the numbers entered in the first line. Format as `Name(string/char[] Type) monthOfBirth(int Type) dayOfBirth(int Type) yearOfBirth(int type)`, separated by spaces. Each data is followed by a new line until all inputs are complete. e.g. `Kim 1 1 1900`. 

<br/>

### Output  

- Output the name and date of birth on the first line.  
  e.g. `Lee born on 2/29/2004`
- Output the date of birth for the 100th day on the second line.  
  e.g. `100th day: 6/7/2004`
- Output the first birthday on the third line.  
  e.g. `First birthday: 2/28/2005`
- Output the date when started elementary school on the 4th line.  
  e.g. `First school day: 3/1/2011`
- After outputting each baby's information, leave a blank line, then re-enter and re-output the next baby's information.

<br/>

### Error handling

- When the month is less than 1 or more than 12, output `"Error: Month must be 1-12"`.
- When the day is less than 1 or exceeds the maximum number of days in the month, output `"Error: Day is out of range for current month and year"`.
- When the year is less than 1900 or more than 2100, output `"Error: Year must be >= 1900 and <= 2100"`.
- Check the entered dates in order of `month, day, and year` for any errors.
- If multiple errors occur, `only output the first error message` in sequence.
- After outputting the error message, `set the birthday date to the default date (1/1/1900)`.

### Other tips
- When reading part of the input through `cin` and wanting to skip the subsequent part, the cache can be cleared using `cin.ignore(std::numeric_limits< streamsize >::max(), '\n');`.
- If your compiler version does not support the use of `std::numeric_limits`, you can replace it with a sufficiently large number, such as 1024.
<br/>

## 2. Scoring Criteria (5 points in total):

- Uploaded `Baby.h`, `Date.h` and `main.cpp` three files with no compilation errors: 1 point
- The cpp file calls two header files and declares the given functions: 2 point
- The result is correct: 2 point

<br/>

## 3 Example(Red font for input, blue font for output):
![image](https://github.com/chyh001228/images/blob/main/w4q2.png)

**Input:**

```
5
Kim 2 29 2004
Jane 13 24 1997
Lee 9 35 2012
Luke 11 14 1857
Roy 20 50 3000
```

**Output:**

```
Kim born on 2/29/2004
100th day: 6/7/2004
First birthday: 2/28/2005
First school day: 3/1/2011

Error: Month must be 1-12
Jane born on 1/1/1900
100th day: 4/10/1900
First birthday: 1/1/1901
First school day: 3/1/1907

Error: Day is out of range for current month and year
Lee born on 1/1/1900
100th day: 4/10/1900
First birthday: 1/1/1901
First school day: 3/1/1907

Error: Year must be >= 1900 and <= 2100
Luke born on 1/1/1900
100th day: 4/10/1900
First birthday: 1/1/1901
First school day: 3/1/1907

Error: Month must be 1-12
Roy born on 1/1/1900
100th day: 4/10/1900
First birthday: 1/1/1901
First school day: 3/1/1907
```

**Actual results:**  
![image](https://github.com/chyh001228/images/blob/main/w4q2_c.png)  

(The deadline is 00:00a.m. on October 5, 2025)

<img src="https://cdn.imweb.me/upload/S201906178853c3e170808/c5d876d707352.jpg" width=30% align=center />
