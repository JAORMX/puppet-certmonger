# Certmonger puppet module

This puppet modules enable some resources and providers that you can use to
request certificates using certmonger.

## Request a certificate from IPA using the certmonger provider

This will create a certificate request with the given hostname (which will be
used in the subject as the CN) and the given principal. It will use the key
specified by 'keyfile'. And if it succeeds it will track the certificate where
'certfile' specifies the resource to do so.

```puppet
  certmonger_certificate { 'my-cert':
    ensure    => 'present',
    ca        => 'IPA'
    certfile  => '/path/to/certs/my-cert.pem',
    keyfile   => '/path/to/certs/my-key.pem',
    hostname  => 'hostname.example.com'
    principal => 'HTTP/hostname.example.com',
  }
```

If the certificate already exists it will simply take the values and add it to
the resource catalog. However, you can tell the provider to resubmit the
certificate if it already exists. This is done by setting the 'force_resubmit'
flag. Currently the aforementioned flag is needed if the parameters for the
request have changed and you wish to resubmit it.

If, for some reason, the CA rejects your request, you can still see the
certificate resource, and the status will reflect the rejection. So, when
viewing the resource, you'll see the following:

```puppet
  certmonger_certificate { 'my-cert':
    ensure      => 'present',
    ca          => 'local'
    certbackend => 'FILE',
    certfile    => '/path/to/certs/my-cert.pem',
    keybackend  => 'FILE',
    keyfile     => '/path/to/certs/my-key.pem',
    status      => 'CA_REJECTED',
  }
```

The default behavior is to throw an error if the CA rejects the request. But
errors can be ignored with the 'ignore_ca_errors' parameter.

One can also automatically stop tracking the certificate request if it's
rejected by the CA. This is done by setting the 'cleanup_on_error' flag.

## Contributing
* Fork it
* Create a topic branch
* Make your changes
* Submit a PR

## Acknowledgements

Thanks to:
* Rob Crittenden for his work on https://github.com/rcritten/puppet-certmonger
  and his helpful reviews
* Github user earsdown for his work on the original version of this module.
