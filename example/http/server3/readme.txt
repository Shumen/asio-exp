http://permalink.gmane.org/gmane.comp.lib.boost.asio.user/3835


Feature 1 is the difference between stopping and aborting the server: when 
the server is stopped receiving is disabled but all outstanding processing 
or sending will be completed. In contrary when the server is aborted it 
will stop as fast as possible (io_service.stop is being called).

Feature 2 is a timeout functionality: when a request is not succesfully 
read within a certain amount of milliseconds the socket of the connection 
will be closed. In case an answer is being sent back (either a normal 
answer or a stock answer) this sending must be completed within another 
certain amount of milliseconds or the socket will be closed.

In case I understood some former postings of this mailing list correctly 
the use of strand in the original http server3 example is actually not 
required. With the introduction of feature 2 the use of strand actually 
becomes a requirement because now several handlers could be called at 
once.
