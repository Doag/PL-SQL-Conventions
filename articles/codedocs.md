# Code Documentation

Code documentation is done within the package body. If done the following way, the documentation can be extracted any time you want (just like with Javadoc). If you want to prevent the extraction, please use ! instead of * as seen in this example:
```
/*!
 * Internal logging procedure.
 * Requires Logger to be installed only while developing.
 *
 *
 * @author Martin D'Souza
 * @created 17-Aug-2015
 * @param p_message Item to log
 * @param p_scope Logger scope
 */
```

Need more examples? Have a look here:
[Logging](logging.md)
