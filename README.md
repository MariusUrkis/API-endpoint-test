# API-endpoint-test
Testing connectivity to API endpoint

Some application has great integration implementations to connect to the remote API and to execute some actions. However there are different conditions when such connection simply does not work for one or another reason (for example expired SSL certifiicate, etc.) and application implementation do not provide good diagnostic for such an error. 
Here is some code to do connection with some diagnostic output.
## Perl
Simulating connection from RTIR to MISP, following RTIR MISP extension implementation
```perl
#!/usr/bin/perl
use strict;
use warnings;

use LWP::UserAgent;
use JSON;
use Data::Dumper qw(Dumper);
use POSIX 'strftime';
my $threedaysago = strftime "%Y-%m-%d", localtime(time - 3 * 24 * 60 * 60);

my $url = $ARGV[0];
my $ApiKeyAuth = $ARGV[1];

my $ua = LWP::UserAgent->new(ssl_opts => { verify_hostname => 0 });
my $default_headers = HTTP::Headers->new(
    'Authorization' => $ApiKeyAuth,
    'Accept'        => 'application/json',
    'Content-Type'  => 'application/json',
);
$ua->default_headers( $default_headers );



my $args = { "searchDatefrom" => $threedaysago };
my $json = encode_json( $args );

my $response = $ua->post($url . '/events/index', Content => $json);
print Dumper($response);
```
