# Consolidation\Log

Improved Psr-3 / Psr\Log logger based on Symfony Console components.

## Component Status

Proposed implementation for review

## Motivation

Consolication\Log provides a Psr-3 compatible logger that provides styled log output to the standard error (stderr) stream. By default, styling is provided by the SymfonyStyle class from the Symfony Console component; however, alternative stylers may be provided if desired.

## Usage
```
$logger = new \Consolidation\Log\Logger();
$logger->warning('The file {name} does not exist.', ['name' => $filename]);
```
n.b. Substitution of replacements, such as `{name}` in the example above, it not required by Psr-3' however, this is often done.

## Comparison to Existing Solutions

Many Symfony Console compoenents use SymfonyStyle to format their output messages. This helper class has methods named things like `success` and `warning`, making it seem like a natural choice for reporting status.

However, in practice it is much more convenient to use an actual Psr-3 logger for logging. Doing this allows a Symfony Console component to call an external library that may not need to depend on Symfony Style.  Having the Psr\Log\LoggerInterface serve as the only shared IO-related interface in common between the console tool and the libraries it depends on promots loose coupling, allowing said libraries to be re-used in other contexts which may wish to log in different ways.

Symfony Console provides the ConsoleLogger to fill this need; however, ConsoleLogger does not provide any facility for styling output, leaving SymfonyStyle as the preferred logging mechanism for style-conscienscious console coders.

Consolidation\Log provides the benefits of both classes, allowing for code that both behaved technically correctly (redirecting to stderr) without sacrificing on style.

## Future

- Log multiplexer: Log output to the console and also some other Psr-3 logger, such as Monolog.
- SymfonyStyleAdapter: Attach a SymfonyStyle and a Consolidation\Log\Logger to a SymfonyStyleAdapter; the adapter can then be used wherever a SymfonyStyle would be used, with the logger being used whenever an appropriate log message is called.
