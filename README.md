# telnetlogger

This is a simple program to log login attempts on Telnet (port 23).

It's designed to track the Mirai botnet. Right now (Oct 23, 2016) infected Mirai
machines from around the world are trying to connect to Telnet on every IP address about once
per minute. This program logs both which IP addresses are doing the attempts, and which
passwords they are using.

# Usage

Just run the program in order to see passwords and IP addresses appear on stdout.

    telnetlogger
  
To log the information to files, use the `-p` and `-i` options.

    telnetlogger -p passwds.txt -i ips.txt
  
To listen on another port (for testing and whatnot), use `-l`.

    telnetlogger -l 2323

Note that on many systems, you'll get an "access denied" error message, because programs
that open ports below 1024 need extra priveleges. So you may need to `sudo` the program.

# Compiling

Type `make` or:

    gcc telnetlogger.c -o telnetlogger -lpthread

It'll also compile/run on Windows.

# Output

There are two sample output files, `passwords.txt` and `ips.txt` that
I generated by running this for the last day.

The program prints the username/password combination one line at a time.

    admin 7ujMko0admin
    root root
    root 54321
    root xmhdipc
    root root
    guest 12345
    root 888888
    root 123456
    admin smcadmin

It doesn't filter duplicates. The easiest way to get rid of duplicates is
just to `sort`/`uniq` the output.

    sort passwords.txt | uniq
  
The IP addresses work the same way as the passwords, with one per line.

    153.99.123.134
    114.35.226.25
    114.35.226.25
    1.52.107.87
    2001:db8:a0b:12f0::1
    114.35.226.25
    1.52.107.87
    1.52.107.87
    153.99.123.134
  
Note that IPv6 is supported. Also note that you'll get lots of duplicates, 
so you'll be doing `sort`/`uniq` a lot in order to reduce the list size. The
duplicates will make it easier to count how often individual IP address's attempt
to connection. Thus, you can run the following:

    sort ips.txt | uniq -c | sort -n
  
This produces results like:

     69 187.136.91.145
     75 79.115.23.228
     90 178.220.226.25
     94 153.99.123.134
    120 171.83.163.1
    168 111.73.37.102
    169 122.96.31.201
    414 119.121.165.137
    


  
