wildmat patch version 0.2 for qmail 1.02
Mark Delany <markd@mira.net.au>
19971203

Changes:
--------
0.1	Initial code
0.2	Fixed buglet relating to systems that had no badmailfrom file
	but do have a badmailpattern file

While the 'badmailfrom' provides some ability to block spam it is
fairly restricted as the match must be exact on either the full string
or the domain. This means that it's very difficult to block the
1234567@aol.com type addresses that some spammers are employing as you
potentially require a large number of entries in 'badmailfrom'.

This patch provides the ability to use simple patterns to reject mail
from unwanted envelope sender addresses. Naturally all such methods
are of limited use against spam as a determined spammer cannot be
stopped on the current Internet, but it does help until the time comes
that we can really stop spammers.

The wildmat patch introduces a new control file called
'badmailpatterns' and is used by qmail-smtpd in conjunction with
'badmailfrom'. You should continue to use 'badmailfrom' when you can
as this is much more CPU-efficient than 'badmailpatterns'.

For those familiar with INN, the wildmat patch uses the wildmat()
routine out of INN and evaluates in the same way. Namely that the
envelope sender is pushed thru all patterns and the final match or
non-match is used to determine whether to reject the mail. It's
implemented this way so that 'not' patterns work.

Here is a sample 'badmailpatterns' file:

*@earthlink.net
!fred@earthlink.net
[0-9][0-9][0-9][0-9][0-9][0-9]@[0-9][0-9][0-9][0-9].com
answerme@save*

This file stops all mail from Earthlink except from
fred@earthlink.net. It also stops all mail with addresses like:
123456@1234.com and answerme@savetrees.com

This patch does not update the documentation or qmail-showctl.

Thanks to Rich Salz for providing wildmat.c by way of the INN
distribution. wildmat.c is fast, small and completely self-contained.

--
