# W CMake znak # rozpoczyna komentarz; reszta wiersza jest pomijana przez konfigurator.
# Ten plik jest instrukcją dla CMake: opisuje wymagane narzędzia,
# pliki źródłowe, biblioteki Qt oraz sposób utworzenia programu Portfel.exe.
# Ustala najstarszą obsługiwaną wersję CMake potrzebną do konfiguracji Qt.
cmake_minimum_required(VERSION 3.21)

# Nadaje projektowi nazwę i informuje CMake, że będzie kompilowany kod C++.
project(Portfel LANGUAGES CXX)

# Wymaga standardu C++17 i zabrania kompilatorowi używania rozszerzeń spoza standardu.
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Wyszukuje Qt 6 i udostępnia moduł Widgets potrzebny do okien i kontrolek.
find_package(Qt6 REQUIRED COMPONENTS Widgets)

# Włącza typowe ustawienia Qt, w tym automatyczne uruchamianie narzędzi MOC i UIC.
qt_standard_project_setup()

# Tworzy program wykonywalny Portfel z plików aplikacji.
qt_add_executable(Portfel
    src/main.cpp
    src/models/Transaction.h
    src/models/Budget.h
    src/services/FinanceManager.h
    src/services/FinanceManager.cpp
    src/services/JsonStorage.h
    src/services/JsonStorage.cpp
    src/ui/AddTransactionDialog.h
    src/ui/AddTransactionDialog.cpp
    src/ui/BudgetDialog.h
    src/ui/BudgetDialog.cpp
    src/ui/ExpenseCharts.h
    src/ui/ExpenseCharts.cpp
    src/ui/MainWindow.h
    src/ui/MainWindow.cpp
)

# Łączy aplikację z biblioteką Qt Widgets.
target_link_libraries(Portfel PRIVATE Qt6::Widgets)

# Wymusza kodowanie UTF-8 dla polskich znaków w kompilatorze MSVC.
if(MSVC)
    target_compile_options(Portfel PRIVATE /utf-8)
endif()

# Na Windowsie uruchamia program bez dodatkowego okna konsoli.
set_target_properties(Portfel PROPERTIES
    WIN32_EXECUTABLE ON
)
