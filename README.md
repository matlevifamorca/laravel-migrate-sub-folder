# laravel-migrate-sub-folder
This is a console command that can be use to migrate all your migration files even if it's on the sub folder.
<p>Step 1 - <b>Run</b> this command <b>php artinsan make:console MigrateAll</b></p>
<p>Step 2 - Open <b>MigrateAll.php</b> file inside the folder <b>app\Console\Commands\MigrateAll</b></p>
<p>Step 3 - <b>Copy</b> the <b>code below</b> and <b>paste</b> it <b>on MigrateAll.php</b> under "class MigrateAll extends Command{"</p>
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'migrate:all';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Allowed migration through folder.';

    /**
     * The path where the migration folder is located.
     *
     * @var string
     */
    protected $migrationFolder;
    protected $defaultPath;

    /*** Create a new command instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->migrationFolder = "\database\migrations";
        $this->defaultPath = base_path() . $this->migrationFolder;
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return mixed
     */
    public function handle()
    {
        // Loop Through All Folders
        if($this->hasSubfolder($this->defaultPath)){
            $this->getSubFolder($this->defaultPath);
        }
    }


    /**
     * Loop through all sub folder under migration folder
     *
     * @return migration
     */
    public function getSubFolder($path){
        foreach(glob($path . '\*', GLOB_ONLYDIR) as $dir) {
            $this->migrate($dir);
        }
    }


    /**
     * Check if current folder has a sub folder
     *
     * @return migration
     */
    public function hasSubfolder($path){
        return count(glob($path . '\*', GLOB_ONLYDIR));
    }

    /**
     * Migrate current folder
     *
     * @return migration
     */
    protected function migrate($path)
    {
        $basePath = $path;
        $currentFolder = str_replace(base_path() . '\\', "", $path);
        
        $this->line("Folder: " . $currentFolder);
        // Migrate current folder
        $this->call('migrate', [
            '--path' => $currentFolder,
        ]);
        // Check sub folder if there something to migrate
        if($this->hasSubfolder($basePath)){
            $this->getSubFolder($basePath);
        }
    }
<p>Step 4 - Go to folder <b>app\Console</b> and open <b>Kernel.php</b></p>
<p>Step 5 - Add <b>Commands\MigrateAll:class</b> in protected $commands</p>
