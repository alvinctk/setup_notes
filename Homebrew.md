


[Homebrew creates log files in your Library folder at:
~/Library/Logs/Homebrew/](https://apple.stackexchange.com/questions/83827/where-does-homebrew-log/83839)

> You can view the log files by holding down Option and using the Finder menu item: Go > Library, then navigating to Logs > Homebrew.
> 
> Alternatively, you can use the Console.app application to browse to the log files.
> 
> Homebrew History
> 
> The default creation of individual log files was added during 2013 to Homebrew.
> 
> Homebrew issue #10430 talks about logging and build errors. The issue report mentions no log file is kept but explains that a log can be created with the command format:
> 
> brew install <formula> 2>&1 | tee install.log


