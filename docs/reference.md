# API Reference 

The "Unofficial" AO3 API works by extracting dating from Archive of Our Own HTML files and returns the requested data. This package is divided in 9 core modules: session, works, chapters, users, series, search, comments, extra, and utils.

## Session

Since not all interactions or works require an account, the session module isn't required, but it can help build a more robust application. If you've used Archive of Our Own for some time, you may know that certain works require you to be logged in to view them. If you want access to all your bookmarks, for example, you'll want to start by storing your credentials in the `AO3.Session` object.

### Authentication

The API uses your AO3 username and password to authenticate requests. These are the same credentials you use to log into to [https://archiveofourown.org/](https://archiveofourown.org/), so keep this secure! If you are writing an app, we recommend writing code that prompts and stores these credentials to avoid hard-coding them.
```
import AO3
session = AO3.Session("username", "password")
```

If you want to leave a comment or kudos anonymously, you can use `AO3.GuestSession`. You won't have to login, but you also won't have access to restricted content or your account. 
```
import AO3
session = AO3.GuestSession()
```

### Kudos

Leave Kudos on a work. Depending on your `session` type, this will either leave the kudos as your user, or anonymously. 
```
print(session.kudos(AO3.Work(15250491, load=False)))
True
```

If the work is private and you are using a Guest Session, you will get an error:
```
print(session.kudos(AO3.Work(15250491, load=False)))
{'errors': {'guest_on_restricted': ["^You can't leave guest kudos on a restricted work."]}}
```

If you are authenticated and have already left kudos, it will return `False`:
```
print(session.kudos(AO3.Work(15250491, load=False)))
False
```

### Bookmarks

There are a few ways to interact with your bookmarks. Each of them require you to be authenticated or you will get the `AttributeError:` `'GuestSession' object has no attribute 'bookmarks'`. So before you begin, make sure your `session` type is authenticated with your username and password. 


#### bookmarks 

Returns the total number of bookmarks for the user. 
```
print(f"Bookmarks: {session.bookmarks}")
Bookmarks: 67
```

### get_bookmarks
Returns a list of bookmarks that include the `workid`, `workname`, and `authors` of each work. 
```
session.get_bookmarks()
[<Work [Etude in Female Friendship Minor n#4]>, <Work [(Harry Potter and) The Predicament with Piddlewink's Peonies]>, <Work [116]>, <Work [An Unexpected Otter]>, <Work [knock me out / knock me down]>, <Work [Advanced Floriography]>, <Work [It's Nice to Have a Friend]>, <Work [Chasing Determined Ghosts]>, <Work [Compendium]>]
```

Since the data is stored in a list of tuples, you can use code to extract any further data you may want.  For example: 
```
user_bookmarks = session.get_bookmarks()
work_ids = []
for bookmark in user_bookmarks:
	work_ids.append(bookmark.id)
print(work_ids)
[38022421, 37587907, 32226049, 15250491, 32222131, 26537827, 39017277, 38865429, 27857342]
```
