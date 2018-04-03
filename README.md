# Typhoon VIPER template

![Demonstation](https://github.com/askarataev/typhoon-viper/blob/dev/code/Resources/Demonstration.png)

[![Language](https://img.shields.io/badge/Language-Liquid-blue.svg)]()
[![License](https://img.shields.io/badge/License-MIT-brightgreen.svg)]()
[![Type](https://img.shields.io/badge/Type-Template-orange.svg)]()


This repository contains a `Swift-VIPER` template for `Generamba`. The template supports automatic generation of the `Typhoon` configurator for the `VIPER` module.


## Generamba

[Generamba](https://github.com/rambler-digital-solutions/Generamba) is a code generator made for working with `Xcode`. Primarily it is designed to generate `VIPER` modules but it is quite easy to customize it for generation of any other classes.

## Template install

* Run `generamba setup` in the project root folder. This command helps to create `Rambafile` that defines all configuration needed to generate code. You can modify this file directly in future.

* Add `typhoon-viper` template to the generated `Rambafile`. Just paste this line to template section:
```
 - { name: 'typhoon-viper', git: 'https://github.com/askarataev/typhoon-viper.git' }
```

* Run `generamba template install`. Template will be placed in the `/Templates` folder of your current project.

* Run `generamba gen typhoon-viper [TEMPLATE_NAME]`. It creates a module with specific name from `typhoon-viper` template.


## Typhoon configurator

After the `Generamba` is finished, the `Typhoon` configurator will be generated. The configurator will look like:
```Swift
//
//  DemonstrationDemonstrationModuleConfigurator.swift
//  typhoon-viper-demo
//
//  Created by Каратаев Алексей on 03/04/2018.
//  Copyright © 2018 Apptolab. All rights reserved.
//

import UIKit
import Typhoon

class DemonstrationModuleConfigurator: TyphoonAssembly {

    @objc public dynamic func configureDemonstrationViewController() -> Any {
        return TyphoonDefinition.withClass(DemonstrationViewController.self) {
            definition in
            definition?.injectProperty(
                Selector(("output")),
                with: self.configureDemonstrationPresenter()
            )
        }
    }

    @objc public dynamic func configureDemonstrationPresenter() -> Any {
        return TyphoonDefinition.withClass(DemonstrationPresenter.self) {
            definition in
            definition?.injectProperty(
                #selector(getter: UITouch.view),
                with: self.configureDemonstrationViewController()
            )
            definition?.injectProperty(
                Selector(("interactor")),
                with: self.configureDemonstrationInteractor()
            )
            definition?.injectProperty(
                Selector(("router")),
                with: self.configureDemonstrationRouter()
            )
        }
    }

    @objc public dynamic func configureDemonstrationRouter() -> Any {
        return TyphoonDefinition.withClass(DemonstrationRouter.self)
    }

    @objc public dynamic func configureDemonstrationInteractor() -> Any {
        return TyphoonDefinition.withClass(DemonstrationInteractor.self) {
            definition in
            definition?.injectProperty(
                Selector(("output")),
                with: self.configureDemonstrationPresenter()
            )
        }
    }
}
```
