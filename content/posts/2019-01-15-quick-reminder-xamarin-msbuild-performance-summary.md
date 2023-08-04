+++ 
date = 2019-01-15T20:10:00+00:00
title = "Quick Reminder: Xamarin - MSBuild Perfomance Summary"
aliases = ['/2019/01/15/quick-reminder-xamarin-msbuild-performance-summary', '/2019/01/15/quick-reminder-xamarin-msbuild-performance-summary.html']
+++

Is your Xamarin project taking a long time to build? Check which process is taking longer with Performance Summary:

`msbuild yourDroidOrIOS.csproj /clp:PerformanceSummary`

Most of the time is Aapt (on Xamarin Android) because of huge png files.