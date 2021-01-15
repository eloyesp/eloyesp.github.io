---
---

Este tiempo estuve escribiendo código muy variado, con lenguajes que no uso
mucho, probando nuevas cosas.

Por un lado, escribí una función en **MySQL**, en medio de una query mucho más
complicada de lo que hubiera querido, lo que hace es expresar un numero entero, que está expresado en centavos de euro o en satoshis de bitcoin de una manera legible y a la vez, redondearlo, para tener valores aproximados.

~~~ sql
delimiter $$
DROP FUNCTION IF EXISTS MONEY$$
CREATE FUNCTION MONEY (amount BIGINT, currency CHAR(5))
RETURNS FLOAT DETERMINISTIC
IF (amount IS NULL OR amount = 0)
THEN RETURN 0;
ELSE
  BEGIN
    DECLARE significant_digits INT;
    DECLARE denominator INT;
    DECLARE decimal_amount FLOAT;
    CASE currency
    WHEN 'EUR' THEN
      IF amount > 800000
      THEN SET significant_digits = 2;
      ELSE SET significant_digits = 1;
      END IF;
      SET denominator = 100;
    ELSE
      SET significant_digits = 3;
      SET denominator = 100000000;
    END CASE;
    SET decimal_amount = amount / denominator;
    RETURN ROUND(decimal_amount,
                 significant_digits - 1 - FLOOR(LOG10(ABS(decimal_amount))));
  END;
END IF
$$
delimiter ;
~~~

También estuve, con ayuda de mi hermano, incursionando en **java**, poniendo mi granito de arena para hacer bitcoin un poco más amigable en Argentina. En eso escribí un [pull request][1], que implementa la API de CryptoCompare en bitcoin wallet usando en particular, la siguiente función, que quizás es muy simple, pero a mi me costó sangre sudor y lagrimas.

~~~ java
public List<ExchangeRateEntry> parse(final BufferedSource jsonSource) throws IOException {
    final Type type = Types.newParameterizedType(Map.class, String.class, String.class);
    final JsonAdapter<Map<String,String>> adapter = moshi.adapter(type);
    final Map<String,String> rates = adapter.fromJson(jsonSource);
    final List<ExchangeRateEntry> result = new ArrayList<>(rates.size());

    for (Map.Entry<String, String> entry : rates.entrySet()) {
        final String symbol = entry.getKey().toUpperCase(Locale.US);
        try {
          final Fiat rate = Fiat.parseFiatInexact(symbol, entry.getValue());
          if (rate.signum() > 0)
            result.add(new ExchangeRateEntry(SOURCE, new ExchangeRate(rate)));
        } catch (final ArithmeticException x) {
          log.warn("problem parsing {} exchange rate from {}: {}", symbol, URL, x.getMessage());
        }
    }
    return result;
}
~~~

Ya hace más de un mes, me puse a generar un template en `**xml**` de un feed rss de
[un podcast][2], que me costó creo mucho más de lo que debería:

~~~ xml
{% raw %}<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.title | xml_escape }}</title>
    <description>{{ site.description | xml_escape }}</description>
    <link>{{ site.url }}{{ site.baseurl }}/</link>
    <image>
      <title>{{ site.title | xml_escape }}</title>
      <url>{{ site.url }}{{ site.baseurl }}/{{ site.logo_image }}</url>
      <link>{{ site.url }}{{ site.baseurl }}</link>
    </image>
    <atom:link href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" rel="self" type="application/rss+xml"/>
    <pubDate>{{ site.time | date_to_rfc822 }}</pubDate>
    <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
    {% for post in site.posts limit:10 %}
      <item>
        <title>{{ post.title | xml_escape }}</title>
        <description>{{ post.content | xml_escape }}</description>
        <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
        <link>{{ post.url | prepend: site.baseurl | prepend: site.url }}</link>
        <guid isPermaLink="true">{{ post.url | prepend: site.baseurl | prepend: site.url }}</guid>
        {% if post.file %}<category>Podcasts</category>
        <enclosure type="audio/mpeg" url="{{ post.file }}"{% if post.length %} length="{{ post.length }}"{% endif %}/>
        {% if post.chapters %}<psc:chapters version="1.2" xmlns:psc="http://podlove.org/simple-chapters">
          {% for chapter in post.chapters %}<psc:chapter start="{{ chapter.start }}" title="{{ chapter.title | xml_escape }}" />
          {% endfor %}
        </psc:chapters>
        {% endif %}
        {% endif %}
        {% for tag in post.tags %}<category>{{ tag | xml_escape }}</category>
        {% endfor %}
        {% for cat in post.categories %}<category>{{ cat | xml_escape }}</category>
        {% endfor %}
      </item>
    {% endfor %}
  </channel>
</rss>{% endraw %}
~~~

Hoy estuve mucho rato escribiendo un hermoso script de **bash** que informa sobre
los archivos en un directorio, de forma recursiva, organizándolos por
extensión, intentando responder a una pregunta en [Unix&Linux][3]:

~~~ bash
set -e
# set -x

folder=$1
counter=$(tempfile)

# List file extensions
list_extensions() {
  find "$folder" -type f |
  while read filename
  do
    basename=${filename##*/}
    ext=${basename##*.}
    echo ${ext,,}  # downcase extensions to prevent duplicates
  done |
  sort -u
}

list_extensions |
while read extension
do
  size=$(find "$folder" -type f -iname "*.$extension" -fprintf $counter . -print0 |
    du -hc --files0-from=- | tail -n 1 | sed -E 's/\s+total//')
  count=$( wc -c < $counter )
  printf "*.%-10s\t%6s files\t%10s\n" "$extension" "$count" "$size"
done

rm $counter
~~~

También estuve incursionando muy poquito en `C`, porque estuve compilando el
código del atreus, pero todavía no he conseguido nada aceptable en ese ámbito.
Estuve metiendo un poco de esfuerzo en **f-droid**, [intentando desbloquear][4] una
aplicación que no se actualiza. Un tiempo atrás estuve haciendo [una incursión
que quedó a medias][5] en rubygems.

Por último, aunque no menos importante, también estuve haciendo intentos de
poemítas, o textos cuidados, con sentidos internos y juegos de lenguaje, así
que puedo decir que aunque vengo medio perdido últimamente, vengo explorando,
buscando, sin mucho éxito un lugar cómodo.

[1]: https://github.com/bitcoin-wallet/bitcoin-wallet/pull/658
[2]: https://eloyesp.gitlab.io/desde-el-barro/
[3]: https://unix.stackexchange.com/a/629167/87725
[4]: https://gitlab.com/fdroid/fdroiddata/-/merge_requests/6058
[5]: https://github.com/rubygems/rubygems/pull/4017
