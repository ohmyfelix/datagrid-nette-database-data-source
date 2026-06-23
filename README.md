![](https://heatbadger.now.sh/github/readme/contributte/datagrid-nette-database-data-source/)

<p align=center>
  <a href="https://travis-ci.org/contributte/datagrid-nette-database-data-source"><img src="https://img.shields.io/travis/contributte/datagrid-nette-database-data-source.svg?style=flat-square"></a>
  <a href="https://packagist.org/packages/ublaboo/datagrid-nette-database-data-source"><img src="https://badgen.net/packagist/dm/ublaboo/datagrid-nette-database-data-source"></a>
  <a href="https://packagist.org/packages/ublaboo/datagrid-nette-database-data-source"><img src="https://badgen.net/packagist/v/ublaboo/datagrid-nette-database-data-source"></a>
</p>
<p align=center>
  <a href="https://packagist.org/packages/ublaboo/datagrid-nette-database-data-source"><img src="https://badgen.net/packagist/php/ublaboo/datagrid-nette-database-data-source"></a>
  <a href="https://github.com/contributte/datagrid-nette-database-data-source"><img src="https://badgen.net/github/license/contributte/datagrid-nette-database-data-source"></a>
  <a href="https://bit.ly/ctteg"><img src="https://badgen.net/badge/support/gitter/cyan"></a>
  <a href="https://bit.ly/cttfo"><img src="https://badgen.net/badge/support/forum/yellow"></a>
  <a href="https://contributte.org/partners.html"><img src="https://badgen.net/badge/sponsor/donations/F96854"></a>
</p>

<p align=center>
Website 🚀 <a href="https://contributte.org">contributte.org</a> | Contact 👨🏻‍💻 <a href="https://paveljanda.com">paveljanda.com</a>, <a href="https://f3l1x.io">f3l1x.io</a> | Twitter 🐦 <a href="https://twitter.com/contributte">@contributte</a>
</p>

Nette Database data source adapter for [ublaboo/datagrid](https://github.com/contributte/datagrid), useful for grids backed by custom SQL queries.

## Versions

| State  | Version | Branch | PHP  |
|--------|---------|--------|------|
| dev    | ~3.0.0  | master | ^7.2 |
| stable | ~2.0.0  | master | ^7.2 |
| stable | ~1.1.0  | master | ^5.6 |

## Installation

To install latest version of `ublaboo/datagrid-nette-database-data-source` use [Composer](https://getcomposer.org).

```bash
composer require ublaboo/datagrid-nette-database-data-source
```

## Usage

```php
/**
 * @var Nette\Database\Context
 * @inject
 */
public $ndb;


public function createComponentNetteGrid($name)
{
	/**
	 * @type Ublaboo\DataGrid\DataGrid
	 */
	$grid = new DataGrid($this, $name);

	$query =
		'SELECT p.*, GROUP_CONCAT(v.code SEPARATOR ", ") AS variants
		FROM product p
		LEFT JOIN product_variant p_v
			ON p_v.product_id = p.id
		WHERE p.deleted IS NULL
			AND (product.status = ? OR product.status = ?)';

	$params = [1, 2];

	/**
	 * @var Ublaboo\NetteDatabaseDataSource\NetteDatabaseDataSource
	 *
	 * @param Nette\Database\Context
	 * @param $query
	 * @param $params|NULL
	 */
	$datasource = new NetteDatabaseDataSource($this->ndb, $query, $params);

	$grid->setDataSource($datasource);

	$grid->addColumnText('name', 'Name')
		->setSortable();

	$grid->addColumnNumber('id', 'Id')
		->setSortable();

	$grid->addColumnDateTime('created', 'Created');

	$grid->addFilterDateRange('created', 'Created:');

	$grid->addFilterText('name', 'Name and id', ['id', 'name']);

	$grid->addFilterSelect('status', 'Status', ['' => 'All', 1 => 'Online', 0 => 'Ofline', 2 => 'Standby']);

	/**
	 * Etc
	 */
}
```


## Development

See [how to contribute](https://contributte.org) to this package. This package is currently maintained by these authors.

<a href="https://github.com/paveljanda">
    <img width="80" height="80" src="https://avatars0.githubusercontent.com/u/1488874?v=3&s=80">
</a>
<a href="https://github.com/f3l1x">
    <img width="80" height="80" src="https://avatars0.githubusercontent.com/u/538058?v=3&s=80">
</a>

-----

Consider to [support](https://contributte.org/partners) **contributte** development team.
Also thank you for using this package.
