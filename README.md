BaconMenuBundle
===============

[![Codacy Badge](https://api.codacy.com/project/badge/grade/3bf0a73bcb79480985d4f1f80062e9c9)](https://www.codacy.com/app/adan-grg/BaconMenuBundle)
[![Latest Stable Version](https://poser.pugx.org/baconmanager/menu-bundle/v/stable)](https://packagist.org/packages/baconmanager/menu-bundle)
[![License](https://poser.pugx.org/baconmanager/menu-bundle/license)](https://packagist.org/packages/baconmanager/menu-bundle)
[![SensioLabsInsight](https://insight.sensiolabs.com/projects/9d14ac78-8fdc-45a6-9708-d81ac752ad66/mini.png)](https://insight.sensiolabs.com/projects/9d14ac78-8fdc-45a6-9708-d81ac752ad66)

Este bundle é responsavel por Customizar a criação de menus do KnpMenuBundle

## Instalação

Para instalar o bundle basta rodar o seguinte comando abaixo:

```bash
$ composer require baconmanager/menu-bundle
```
Agora adicione os seguintes bundles no arquivo AppKernel.php:

```php
<?php
// app/AppKernel.php
public function registerBundles()
{
    // ...
    new Knp\Bundle\MenuBundle\KnpMenuBundle(),
    new Bacon\Bundle\MenuBundle\BaconMenuBundle(),
    // ...
}
```
## Configuração 

Adicionar as configurações abaixo no arquivo **app/config/config.yml**

```yaml
#Menu
knp_menu:
    # use "twig: false" to disable the Twig extension and the TwigRenderer
    twig:
        template: BaconCoreBundle:partial:menu.html.twig
    #  if true, enables the helper for PHP templates
    templating: false
    # the renderer to use, list is also available by default
    default_renderer: twig
```

## Criando um menu
Para criar um novo menu basta criar uma classe no namespace MyBundle/Menu/Builder como no exemplo abaixo:

Para versão symfony =< 2.7

```php
<?php
// src/AppBundle/Menu/Builder.php
namespace AppBundle\Menu;

use Knp\Menu\FactoryInterface;
use Symfony\Component\DependencyInjection\ContainerAware;

class Builder extends ContainerAware
{
    public function addMenu(FactoryInterface $factory, array $options)
    {
        $menu = $factory->createItem('root');

        $translate = $this->container->get('translator');

        // Menu Catalog
        $menu->addChild($translate->trans('Category'))->setAttribute('icon', '<i class="fa fa-book"></i>');
        $menu[$translate->trans('Category')]->addChild($translate->trans('List'),array('route' => 'admin_category'));
        $menu[$translate->trans('Category')]->addChild($translate->trans('New'),array('route' => 'admin_category_new'));

        return $menu;
    }
}
```

Para versão do symfony >= 2.8

```php
<?php
// src/AppBundle/Menu/Builder.php
namespace AppBundle\Menu;

use Knp\Menu\FactoryInterface;
use Symfony\Component\DependencyInjection\ContainerAwareInterface;
use Symfony\Component\DependencyInjection\ContainerAwareTrait;

class Builder implements ContainerAwareInterface
{
	use ContainerAwareTrait;
	
    public function addMenu(FactoryInterface $factory, array $options)
    {
        $menu = $factory->createItem('root');

        $translate = $this->container->get('translator');

        // Menu Catalog
        $menu->addChild($translate->trans('Category'))->setAttribute('icon', '<i class="fa fa-book"></i>');
        $menu[$translate->trans('Category')]->addChild($translate->trans('List'),array('route' => 'admin_category'));
        $menu[$translate->trans('Category')]->addChild($translate->trans('New'),array('route' => 'admin_category_new'));

        return $menu;
    }
}
```

## Renderizando o menu no layout(twig):

```
{{ bacon_menu_full_render() }}
```
