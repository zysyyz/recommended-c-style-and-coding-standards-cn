## 7. 例子 ##

```

/*
	 * Determine if the sky is blue by checking that it isn't night.
	 * CAVEAT: Only sometimes right.  May return TRUE when the answer
	 * is FALSE.  Consider clouds, eclipses, short days.
	 * NOTE: Uses 'hour' from 'hightime.c'.  Returns 'int' for
	 * compatibility with the old version.
	 */
		int				/* true or false */
	skyblue()
	{
		extern int	hour;		/* current hour of the day */
	
		return (hour >= MORNING && hour <= EVENING);
	}

	/*
	 * Find the last element in the linked list
	 * pointed to by nodep and return a pointer to it.
	 * Return NULL if there is no last element.
	 */
		node_t *
	tail(nodep)
		node_t	*nodep;			/* pointer to head of list */
	{
		register node_t	*np;		/* advances to NULL */
		register node_t	*lp;		/* follows one behind np */
	
		if (nodep == NULL)
			return (NULL);
		for (np = lp = nodep; np != NULL; lp = np, np = np->next)
			;	/* VOID */
		return (lp);
	}

```