# API Reference 

The "Unofficial" AO3 API extracts data from the Archive of Our Own website files (HTML) and returns the requested data. This package is divided into 9 core modules: session, works, chapters, users, series, search, comments, extra, and utils.

## Session

The Session module allows you to log in and interact with your AO3 account. If you've used Archive of Our Own before, you may know that some works require you to be logged in to view them. If you want access to all your bookmarks, you'll want to start by storing your credentials in the `AO3.Session` object. 

Since not all interactions or works require an account, the session module isn't required, but it can help build a more robust application.

### Authentication

The API uses your AO3 username and password to authenticate requests. These are the same credentials you use to log into [https://archiveofourown.org/](https://archiveofourown.org/), so keep these secure! If you are writing an app, we recommend writing code that prompts and stores these credentials to avoid hard-coding them.
```
import AO3
session = AO3.Session("username", "password")
```

If you want to leave a comment or kudos anonymously, you can use `AO3.GuestSession`. You won't have to log in, but you also won't have access to restricted content or your account. 
```
import AO3
session = AO3.GuestSession()
```

### Kudos

Leave Kudos on a work. Depending on your `session` type, this will either leave the kudos as your user or anonymously. 
```
print(session.kudos(AO3.Work(12345678, load=False)))
True
```

If the work is private and you are using a Guest Session, you will get an error:
```
print(session.kudos(AO3.Work(12345678, load=False)))
{'errors': {'guest_on_restricted': ["^You can't leave guest kudos on a restricted work."]}}
```

If you are authenticated and have already left kudos, it will return `False`:
```
print(session.kudos(AO3.Work(12345678, load=False)))
False
```

### Bookmarks

There are a few ways to interact with your bookmarks. Each of them requires you to be logged in, or you will get the `AttributeError:'GuestSession' object has no attribute 'bookmarks'`. So before you begin, make sure your `session` type is authenticated with your username and password. 


#### bookmarks 

Returns the total number of bookmarks for the user as an integer.  
```
print(f"Bookmarks: {session.bookmarks}")
Bookmarks: 9
```

### get_bookmarks
Returns a list of bookmarks that include the `id`, `workname`, and `authors` of each work. 
```
session.get_bookmarks()
[<Work [Frankenstein]>, <Work [Dracula]>, <Work [Jane Eyre]>, <Work [The Time Machine]>, <Work [The Adventures of Huckleberry Finn]>, <Work [Pride and Prejudice]>, <Work [A Christmas Carol]>, <Work [Wuthering Heights]>, <Work [The Wonderful Wizard of Oz]>]
```

Since the data is stored in a list of tuples, you can use code to extract any further data you may want.  For example: 
```
user_bookmarks = session.get_bookmarks()
work_ids = []
for bookmark in user_bookmarks:
	work_ids.append(bookmark.id)
print(work_ids)
[12345678, 22345678, 32345678, 42345678, 52345678, 62345678, 72345678, 82345678, 92345678]
```
