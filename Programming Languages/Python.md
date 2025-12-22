- https://docs.python.org/3/tutorial/
- http://getpython3.com/diveintopython3/index.html
- https://scapy.readthedocs.io/en/latest/introduction.html

# Datatypes
![[Python Datatypes.png]]
## Strings
- **Definition**:
	```python
string1 = "Hello"
string2 = "World"
	```
- **Concatenate Strings**:
  ```python
  print(string1 + string2)
  print("The message is:" + string1 + string2)
  ```
### Using built-in methods of strings
- **upper() and lower()**
  helpful when comparing strings that do not need to be case-sensitive; will return the string in all upper or lower case letters
  ```python
  >>>text = 'Test'
  >>>text.lower()
  'test'
  >>>text.upper()
  'TEST'
  ```
  the original variable will not be altered
- **startswith() and endswith()**
  these methods are used to verify if a string starts or ends with a certain sequence of characters
  ```python
  >>> ipaddr = '10.100.20.5'
  >>> ipaddr.startswith('10')
  True
  >>>ipaddr.startswith('100')
  False
  >>>ipaddr.endswith('.5')
  True
  ```
- **strip()**
  this method returns objects without any spaces at the beginning and end
  ```python
  >>>ipaddr = '  10.100.20.5   '
  >>>ipaddr.strip()
  '10.100.20.5'
  ```
- **isdigit()**
  this method checks if a string consists of digits and returns True or False
  ```python
  >>>ten = '10'
  >>>ten.isdigit()
  True
  >>>bogus= '10a'
  >>>bogus.isdigit()
  False
  ```
- **count()**
  allows you to count the number of single characters or character sequences
  ```python
  >>>octet= '11111000'
  >>>octet.count('1')
  5
  >>>octet.count('111')
  1
  >>>test_string= "What would you wish for if you had three wishes"
  >>>test_string.count('you')
  2
  ```
- **format()**
  used to format the output strings 
  ```python
  >>>ping= 'ping {} verf {}'.format(ipaddr,vrf)
  >>>print(ping)
  ping 8.8.8.8 vrf management
  ```
- **join() and split()**
# Literals
# Integers
# Input() Function
This function reads data entered by the user and returns that data to the program.
>[!info] Deaf Program
>A program that does not get a user's input is a *deaf program*.

```Python
print("Tell me a secret...")
anything = input()
print("Hmm...", anything, "... That is a good secret.")
```
## Input Function with an Argument
You can use the `input` function to give a prompt to the user without the `print` function, like above.
```Python
secret = input("Tell me a secret")
print("Hmmm...", secret, "I will not tell anyone.")
```
# Logical Operators
