### Program description ###
This program will go to The Astrophysical Journal website and retrieve the 'Received' and 'Accepted' dates for a sample of articles accepted in ApJ or ApJL in the previous year, saving them into a spreadsheet.

Inputs: journal name ('ApJ' or 'ApJL'), number of articles to sample per issue 

Outputs: .csv file (open in Excel)


### Quickstart guide ###

1. Navigate to project folder, and activate virtual environment.

```
>> activate
[env] >>
```

2. Run test suite to check core functionality.

```
>> runtests
test_writer_creates_file (test_file_writer.FileWriterTests) ... ok
...
test_article_gets_dates (test_article.ArticleTests) ... ok
test_article_handles_different_date_string_format (test_article.ArticleTests) ... ok

----------------------------------------------------------------------
Ran 18 tests in 533.066s

OK
```

3. Run full script in test mode to check overall functionality.

```
>> python script.py test
Running publication-dates version 1.1

Testing.....
2019November4 2019December23
2019September6 2019December17
2019May29 2019December6

----------------------------------------------------------------------
Sampled  3  articles in:  0.128765085803 hrs
```

4. Debug any errors - target website sometimes changes from year to year, so some code updates may be required, even if tests all pass. Add tests to cover any new issues, and update code to fix the problem.

5. Run full script. As a guideline, it should take a couple of minutes at the start to build the article index, and then the script should start printing out results to screen. 

```
>> python script.py
Running publication-dates version 1.1

2019October25 2019December21
2019October29 2019December20
2019November27 2019December18
2019November18 2019December17
Some error occurred (URL ' http://iopscience.iop.org/article/10.3847/1538-4357/ab1b2a ' not available?). Skipping.
2019May6 2019December17
2019October14 2019December3
2019November10 2019December1
2019September25 2019November15

----------------------------------------------------------------------
Sampled 8 articles in: 0.58765085803 hrs
```


### Technical details ###

Assumptions:  
* There are at least 30 articles per issue
* There are 2 issues per volume
* There are 18 volumes per year (1.5 per month, averaged)

The program will attempt to load the URLs of the first 30 articles in issue 1 of the starting volume. It will retrieve the dates for as many of these as possible, before doing the same for issue 1 of the previous volume, and then issue 1 of the volume before that, etc. It will loop through the previous 18 volumes in this manner, corresponding to the previous year, to return the dates for a sample of at least 500 articles (URLs on the ApJ website are not entirely consistent, so the date retrieval may fail for some articles). 

An article accepted on 1st January will take roughly a month to make it into a published issue, so articles accepted in a given year start appearing around February. Similarly, the January issue of the given year will contain mostly articles accepted in December of the year before that. The program therefore automatically detects the first completely-February volume of the current year (e.g. Volume 836: Number 1, 10 February 2017), and loops back through to the previous February, doing some final filtering to remove articles not accepted in the desired year.

The parameters (number of articles to use per issue, and whether to sample ApJ or ApJL) are defined in the file 'parameters.py', and can be changed by the user. For at least 2012 - 2016 inclusive, there have been 18 volumes of ApJ per year, each with 2 issues, so it is assumed that this pattern will continue and the number of volumes per year (18) is hard-coded in.


### LEGAL DISCLAIMER ###
Please be aware that using this program for anything other than its intended purpose may contravene copyright regulations. The program targets the ApJ and ApJL journals hosted on the IoP website, and text and data mining for non-commercial research are, as of 2014, exceptions to UK copyright law (https://www.gov.uk/guidance/exceptions-to-copyright). Text and data mining may not be permissible in other countries and on other websites, and will probably contravene the terms of use of such sites in any case.
