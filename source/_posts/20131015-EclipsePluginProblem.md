---
title: Eclipse插件开发向导页问题分析
date: 2013-10-15 01:26:10
categories: tools
tags: [eclipse]
---
在开发Eclipse插件过程中，遇到这样的错误：

	The selected wizard could not be started.xxxWizard cannot be cast to org.eclipse.ui.IWorkbenchWizard;

错误分析:

xxxWizard类使用了IWorkbenchWizard接口的init方法而没有实现该接口，我们只需要实现 INewWizard 接口即可（INewWizard接口是IWorkbenchWizard的子接口，而这两个接口又都是IWizard接口的子接口）；

在Eclipse中，向导必须实现接口org.eclipse.jface.wizard.IWizard，如不实现IWizard接口而只继承Wizard类，则还需实现performFinish()方法，因为该方法在Wizard类中是对接口IWizard的抽象实现，即Eclipse提供的org.eclipse.jface.wizard.Wizard类是IWizard的抽象实现；一般我们编写自己的Wizard类既继承Wizard类，且实现INewWizard接口。

向导页必须实现接口org.eclipse.jface.wizard.IWizardPage，因为org.eclipse.jface.wizard.WizardPage 也是 IWizardPage 的一个抽象实现.